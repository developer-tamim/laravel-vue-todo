<template>
    <div class="max-w-4xl p-6 mx-auto">
      <!-- Title -->
      <h1 class="mb-4 text-2xl font-bold text-gray-800">To-do List</h1>

      <!-- Add Task Button -->
      <button
        @click="openModal"
        class="px-4 py-2 text-white transition bg-blue-500 rounded shadow-md hover:bg-blue-600">
        + Add Task
      </button>

      <!-- Table -->
      <div class="mt-4 overflow-x-auto">
        <table class="w-full bg-white border-collapse rounded-md shadow-lg">
          <thead>
            <tr class="text-sm text-gray-700 uppercase bg-gray-100">
              <th class="px-4 py-3 border">SL</th>
              <th class="px-4 py-3 border">Tasks</th>
              <th class="px-4 py-3 border">Description</th>
              <th class="px-4 py-3 border">Status</th>
              <th class="px-4 py-3 border">Action</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(task, index) in tasks" :key="task.id" class="hover:bg-gray-50">
              <td class="px-4 py-3 text-center border">{{ index + 1 }}</td>
              <td class="px-4 py-3 border">
                <span class="font-semibold">{{ task.title }}</span>
                <!-- <div class="text-xs text-gray-500">{{ task.createdAt }}</div> -->
              </td>
              <td class="px-4 py-3 border">{{ task.description }}</td>
              <td class="px-4 py-3 border">
                <span :class="task.status === 'Completed' ? 'text-green-600 font-semibold' : 'text-yellow-600 font-semibold'">
                  {{ task.status }}
                </span>
              </td>
              <td class="flex items-center justify-center px-4 py-3 space-x-3 border">
                <!-- <button @click="viewTask(task)" class="text-blue-500 hover:underline">👁 Show</button> -->
                <button @click="viewTask(task)" class="text-blue-500 hover:underline">
  <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="icon icon-tabler icons-tabler-outline icon-tabler-zoom-scan">
    <path stroke="none" d="M0 0h24v24H0z" fill="none"/>
    <path d="M4 8v-2a2 2 0 0 1 2 -2h2"/>
    <path d="M4 16v2a2 2 0 0 0 2 2h2"/>
    <path d="M16 4h2a2 2 0 0 1 2 2v2"/>
    <path d="M16 20h2a2 2 0 0 0 2 -2v-2"/>
    <path d="M8 11a3 3 0 1 0 6 0a3 3 0 0 0 -6 0"/>
    <path d="M16 16l-2.5 -2.5"/>
  </svg>
</button>

                <!-- <button @click="editTask(task)" class="text-green-500 hover:underline">✏ Edit</button> -->
                <button @click="editTask(task)" class="text-green-500 hover:underline">
  <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="icon icon-tabler icons-tabler-outline icon-tabler-edit">
    <path stroke="none" d="M0 0h24v24H0z" fill="none"/>
    <path d="M7 7h-1a2 2 0 0 0 -2 2v9a2 2 0 0 0 2 2h9a2 2 0 0 0 2 -2v-1"/>
    <path d="M20.385 6.585a2.1 2.1 0 0 0 -2.97 -2.97l-8.415 8.385v3h3l8.385 -8.415z"/>
    <path d="M16 5l3 3"/>
  </svg>
</button>

                <!-- <button @click="deleteTask(task.id)" class="text-red-500 hover:underline">🗑 Delete</button> -->
                <button @click="deleteTask(task.id)" class="text-red-500 hover:underline">
  <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="icon icon-tabler icons-tabler-outline icon-tabler-trash">
    <path stroke="none" d="M0 0h24v24H0z" fill="none"/>
    <path d="M4 7l16 0"/>
    <path d="M10 11l0 6"/>
    <path d="M14 11l0 6"/>
    <path d="M5 7l1 12a2 2 0 0 0 2 2h8a2 2 0 0 0 2 -2l1 -12"/>
    <path d="M9 7v-3a1 1 0 0 1 1 -1h4a1 1 0 0 1 1 1v3"/>
  </svg>
</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>

      <!-- Modal for Adding or Editing Tasks -->
      <div v-if="isModalOpen" class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50">
        <div class="p-6 bg-white rounded-lg shadow-lg w-96">
          <h2 class="mb-4 text-xl font-semibold">{{ isEditMode ? 'Edit Task' : 'Add Task' }}</h2>
          <form @submit.prevent="saveTask">
            <!-- Task Title -->
            <div class="mb-4">
              <label for="taskTitle" class="block text-sm font-medium">Task Title</label>
              <input v-model="taskForm.title" id="taskTitle" type="text" class="w-full p-2 mt-1 border rounded focus:outline-none focus:ring-2 focus:ring-blue-400" required />
            </div>

            <!-- Status Dropdown -->
            <div class="mb-4">
              <label for="taskStatus" class="block text-sm font-medium">Status</label>
              <select v-model="taskForm.status" id="taskStatus" class="w-full p-2 mt-1 border rounded focus:outline-none focus:ring-2 focus:ring-blue-400" required>
                <option value="Pending">Pending</option>
                <option value="Completed">Completed</option>
              </select>
            </div>

            <!-- Description -->
            <div class="mb-4">
              <label for="taskDescription" class="block text-sm font-medium">Description</label>
              <textarea v-model="taskForm.description" id="taskDescription" rows="3" class="w-full p-2 mt-1 border rounded focus:outline-none focus:ring-2 focus:ring-blue-400" required></textarea>
            </div>

            <!-- Buttons -->
            <div class="flex justify-end space-x-3">
              <button @click="closeModal" type="button" class="px-4 py-2 text-gray-700 bg-gray-300 rounded">Cancel</button>
              <button type="submit" class="px-4 py-2 text-white bg-blue-500 rounded hover:bg-blue-600">Save</button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </template>

  <script>
  export default {
    data() {
      return {
        tasks: [],
        isModalOpen: false,
        isEditMode: false,
        taskForm: {
          id: null,
          title: '',
          description: '',
          status: 'Pending',
          createdAt: ''
        }
      };
    },
    methods: {
      openModal() {
        this.isModalOpen = true;
        this.isEditMode = false;
        this.taskForm = { id: null, title: '', description: '', status: 'Pending', createdAt: '' };
      },
      closeModal() {
        this.isModalOpen = false;
      },
      saveTask() {
  if (this.isEditMode) {
    const index = this.tasks.findIndex(task => task.id === this.taskForm.id);
    if (index !== -1) {
      this.tasks[index] = { ...this.taskForm, createdAt: this.tasks[index].createdAt };
    }
  } else {
    const newTask = {
      id: this.tasks.length ? Math.max(...this.tasks.map(t => t.id)) + 1 : 1, // Ensure unique ID
      ...this.taskForm,
      createdAt: new Date().toLocaleString()
    };
    this.tasks.push(newTask);
  }
  this.closeModal();
},
      editTask(task) {
        this.isEditMode = true;
        this.taskForm = { ...task };
        this.openModal();
      },
      viewTask(task) {
        alert(`Task Title: ${task.title}\nDescription: ${task.description}\nStatus: ${task.status}`);
      },
      deleteTask(taskId) {
        this.tasks = this.tasks.filter(task => task.id !== taskId);
      }
    }
  };
  </script>

  <style scoped>
  /* Additional custom styles can be added here */
  </style>
