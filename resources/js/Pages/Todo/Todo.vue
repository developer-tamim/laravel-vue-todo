<template>
    <div class="p-6">
      <h1 class="text-2xl font-bold mb-4">To-do List</h1>
      <button @click="openModal()" class="mb-4 px-4 py-2 bg-blue-500 text-white rounded">Add Task</button>

      <table class="w-full border-collapse border border-gray-300">
        <thead>
          <tr class="bg-gray-200">
            <th class="border p-2">SL</th>
            <th class="border p-2">Tasks</th>
            <th class="border p-2">Description</th>
            <th class="border p-2">Status</th>
            <th class="border p-2">Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(task, index) in tasks" :key="task.id" class="border">
            <td class="border p-2">{{ index + 1 }}</td>
            <td class="border p-2">
              {{ task.title }}
              <div class="text-sm text-gray-500">{{ task.createdAt }}</div>
            </td>
            <td class="border p-2">{{ task.description }}</td>
            <td class="border p-2">{{ task.status }}</td>
            <td class="border p-2 flex space-x-2">
              <button @click="openModal(task)" class="text-blue-500">✏️</button>
              <button @click="viewTask(task)" class="text-green-500">👁️</button>
              <button @click="deleteTask(task.id)" class="text-red-500">🗑️</button>
            </td>
          </tr>
        </tbody>
      </table>

      <div v-if="isModalOpen" class="fixed inset-0 flex items-center justify-center bg-gray-900 bg-opacity-50">
        <div class="bg-white p-6 rounded shadow-lg w-96">
          <h2 class="text-xl font-bold mb-4">{{ editingTask ? 'Edit Task' : 'Add Task' }}</h2>
          <label class="block mb-2">Task Title</label>
          <input v-model="taskForm.title" class="w-full p-2 border rounded mb-2" type="text" />

          <label class="block mb-2">Status</label>
          <select v-model="taskForm.status" class="w-full p-2 border rounded mb-4">
            <option value="Pending">Pending</option>
            <option value="Completed">Completed</option>
          </select>
          
          <label class="block mb-2">Description</label>
          <textarea v-model="taskForm.description" class="w-full p-2 border rounded mb-2"></textarea>


          <div class="flex justify-end space-x-2">
            <button @click="closeModal" class="px-4 py-2 bg-gray-500 text-white rounded">Cancel</button>
            <button @click="saveTask" class="px-4 py-2 bg-blue-500 text-white rounded">Save</button>
          </div>
        </div>
      </div>
    </div>
  </template>

  <script setup>
  import { ref } from 'vue';

  const tasks = ref([]);
  const isModalOpen = ref(false);
  const editingTask = ref(null);
  const taskForm = ref({ title: '', description: '', status: 'Pending', createdAt: '' });

  const openModal = (task = null) => {
    isModalOpen.value = true;
    if (task) {
      editingTask.value = task;
      taskForm.value = { ...task };
    } else {
      editingTask.value = null;
      taskForm.value = { title: '', description: '', status: 'Pending', createdAt: new Date().toLocaleString() };
    }
  };

  const closeModal = () => {
    isModalOpen.value = false;
  };

  const saveTask = () => {
    if (editingTask.value) {
      Object.assign(editingTask.value, taskForm.value);
    } else {
      tasks.value.push({ ...taskForm.value, id: Date.now() });
    }
    closeModal();
  };

  const deleteTask = (id) => {
    tasks.value = tasks.value.filter(task => task.id !== id);
  };

  const viewTask = (task) => {
    alert(`Task: ${task.title}\nDescription: ${task.description}\nStatus: ${task.status}`);
  };
  </script>
