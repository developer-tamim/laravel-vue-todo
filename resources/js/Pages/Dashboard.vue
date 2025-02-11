<script setup>
import AuthenticatedLayout from "@/Layouts/AuthenticatedLayout.vue";
import { Head } from "@inertiajs/vue3";
</script>

<template>

    <Head title="Dashboard" />

    <AuthenticatedLayout>
        <!-- <template #header>
            <h2
                class="text-xl font-semibold leading-tight text-gray-800"
            >
                Dashboard
            </h2>
        </template> -->

        <div class="py-12">
            <div class="mx-auto max-w-7xl sm:px-6 lg:px-8">
                <div class="overflow-hidden bg-white shadow-sm sm:rounded-lg">
                    <div class="p-6 text-gray-900">
                        <!-- Todo Start -->
                        <h1 class="mb-4 text-2xl font-bold">Todo List</h1>
                        <div class="mb-4">
                            <input v-model="newTask" type="text" class="w-5/6 p-2 border border-gray-300 rounded"
                                placeholder="Add new task" @keyup.enter="addTask" />
                            <button class="px-4 py-2 ml-2 font-bold text-white bg-green-500 rounded hover:bg-green-700"
                                @click="addTask">
                                Add
                            </button>
                        </div>
                        <div class="overflow-x-auto">
                            <table class="w-full table-auto">
                                <thead class="bg-gray-100">
                                    <tr>
                                        <th class="w-10 px-4 py-2">SL</th>
                                        <th class="w-64 px-4 py-2">Tasks</th>
                                        <th class="w-24 px-4 py-2 text-center">Action</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr class="bg-white hover:bg-gray-100">
                                        <th class="px-4 py-2">1</th>
                                        <td class="px-4 py-2">{{ task }}</td>
                                        <td class="px-4 py-2 text-center">
                                            <button
                                                class="px-2 py-1 font-bold text-white bg-blue-500 rounded hover:bg-blue-700" @click="editTask(index)">
                                                Edit
                                            </button>
                                            <button
                                                class="px-2 py-1 ml-2 font-bold text-white bg-red-500 rounded hover:bg-red-700" @click="deleteTask(index)">
                                                Delete
                                            </button>
                                        </td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                        <!-- Todo End-->
                    </div>
                </div>
            </div>
        </div>
    </AuthenticatedLayout>
</template>

<script>
export default {
  data() {
    return {
      tasks: ["Task 1", "Task 2", "Task 3"],
      newTask: "",
    };
  },
  methods: {
    addTask() {
      if (this.newTask.trim() !== "") {
        this.tasks.push(this.newTask.trim());
        this.newTask = ""; // Clear the input field after adding the task
      }
    },
    editTask(index) {
      alert(`Editing task: ${this.tasks[index]}`);
      // Implement your edit logic here
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
  },
};
</script>

<style scoped></style>
