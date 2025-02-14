<template>
    <div class="p-6">
      <h1 class="text-2xl font-bold mb-4">To-do List</h1>
      <button @click="openModal('add')" class="mb-4 px-4 py-2 bg-blue-500 text-white rounded">Add Task</button>

      <table class="w-full border-collapse border border-gray-300">
        <thead>
          <tr class="bg-gray-200">
            <th class="p-2">SL</th>
            <th class="border p-2">Tasks</th>
            <th class="border p-2">Description</th>
            <th class="border p-2">Status</th>
            <th class="p-2">Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(task, index) in tasks" :key="task.id" class="border">
            <td class="p-2 flex items-center justify-center">{{ index + 1 }}</td>
            <td class="border p-2">
              {{ task.title }}
              <div class="text-sm text-gray-500">{{ task.created_at }}</div>
            </td>
            <td class="border p-2">{{ task.description }}</td>
            <td class="border p-2">{{ task.status }}</td>
            <td class="p-2 flex items-center justify-center space-x-2">
              <button @click="openModal('view', task)" class="text-green-500">View</button>
              <button @click="openModal('edit', task)" class="text-blue-500">Edit</button>
              <button @click="deleteTask(task.id)" class="text-red-500">Delete</button>
            </td>
          </tr>
        </tbody>
      </table>

      <div v-if="isModalOpen" class="fixed inset-0 flex items-center justify-center bg-gray-900 bg-opacity-50">
        <div class="bg-white p-6 rounded shadow-lg w-96">
          <h2 class="text-xl font-bold mb-4">
            {{ modalMode === 'add' ? 'Add Task' : modalMode === 'edit' ? 'Edit Task' : 'View Task' }}
          </h2>
          <label class="block mb-2">Task Title</label>
          <input v-model="taskForm.title" :readonly="modalMode === 'view'" class="w-full p-2 border rounded mb-2" type="text" />

          <label class="block mb-2">Description</label>
          <textarea v-model="taskForm.description" :readonly="modalMode === 'view'" class="w-full p-2 border rounded mb-2"></textarea>

          <label class="block mb-2">Status</label>
          <select v-model="taskForm.status" :disabled="modalMode === 'view'" class="w-full p-2 border rounded mb-4">
            <option value="Pending">Pending</option>
            <option value="Completed">Completed</option>
          </select>

          <div class="flex justify-end space-x-2">
            <button @click="closeModal" class="px-4 py-2 bg-gray-500 text-white rounded">Close</button>
            <button v-if="modalMode !== 'view'" @click="saveTask" class="px-4 py-2 bg-blue-500 text-white rounded">
              Save
            </button>
          </div>
        </div>
      </div>
    </div>
  </template>

  <script setup>
  import { ref, onMounted } from 'vue';
//   import { Inertia } from '@inertiajs/inertia';

  const tasks = ref([]);
  const isModalOpen = ref(false);
  const modalMode = ref('add');
  const editingTask = ref(null);
  const taskForm = ref({ title: '', description: '', status: 'Pending', created_at: '' });

  const openModal = (mode, task = null) => {
    modalMode.value = mode;
    isModalOpen.value = true;
    if (task) {
      editingTask.value = task;
      taskForm.value = { ...task };
    } else {
      editingTask.value = null;
      taskForm.value = { title: '', description: '', status: 'Pending', created_at: new Date().toLocaleString() };
    }
  };

  const closeModal = () => {
    isModalOpen.value = false;
  };

  const saveTask = () => {
    if (editingTask.value) {
      Inertia.put(route('todo.update', editingTask.value.id), taskForm.value, {
        onSuccess: () => closeModal(),
      });
    } else {
      Inertia.post(route('todo.store'), taskForm.value, {
        onSuccess: () => closeModal(),
      });
    }
  };

  const deleteTask = (id) => {
    if (confirm('Are you sure you want to delete this task?')) {
      Inertia.delete(route('todo.destroy', id), {
        onSuccess: () => closeModal(),
      });
    }
  };

  onMounted(() => {
    Inertia.get(route('todo.index'), {}, {
      onSuccess: (page) => {
        tasks.value = page.props.tasks;
      },
    });
  });
  </script>
