<script>
import AuthenticatedLayout from "@/Layouts/AuthenticatedLayout.vue";
import { Head } from "@inertiajs/vue3";
</script>

<template>

    <Head title="Dashboard" />

    <AuthenticatedLayout>
        <div class="py-12">
            <div class="mx-auto max-w-7xl sm:px-6 lg:px-8">
                <div class="overflow-hidden bg-white shadow-sm sm:rounded-lg">
                    <div class="p-6 text-gray-900">
                        <!-- todo start -->
                        <div class="p-6">
                            <h1 class="mb-4 text-2xl font-bold">To-do List</h1>
                            <button @click="openModal('add')" class="px-4 py-2 mb-4 text-white bg-blue-500 rounded">
                                Add Task
                            </button>

                            <table class="w-full border border-collapse border-gray-300">
                                <thead>
                                    <tr class="bg-gray-200">
                                        <th class="p-2">SL</th>
                                        <th class="p-2 border">Tasks</th>
                                        <th class="p-2 border">Description</th>
                                        <th class="p-2 border">Status</th>
                                        <th class="p-2">Action</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr v-for="(task, index) in tasks" :key="task.id" class="border">
                                        <td class="flex items-center justify-center p-2">
                                            {{ index + 1 }}
                                        </td>
                                        <td class="p-2 border">
                                            {{ task.title }}
                                            <div class="text-sm text-gray-500">
                                                {{ task.createdAt }}
                                            </div>
                                        </td>
                                        <td class="p-2 border">
                                            {{ task.description }}
                                        </td>
                                        <td class="p-2 border">
                                            {{ task.status }}
                                        </td>
                                        <td class="flex items-center justify-center p-2 space-x-2">
                                            <button @click="openModal('view', task)" class="text-green-500">
                                                View
                                            </button>
                                            <button @click="openModal('edit', task)" class="text-blue-500">
                                                Edit
                                            </button>
                                            <button @click="deleteTask(task.id)" class="text-red-500">
                                                Delete
                                            </button>
                                        </td>
                                    </tr>
                                </tbody>
                            </table>

                            <div v-if="isModalOpen"
                                class="fixed inset-0 flex items-center justify-center bg-gray-900 bg-opacity-50">
                                <div class="p-6 bg-white rounded shadow-lg w-96">
                                    <h2 class="mb-4 text-xl font-bold">
                                        {{
                                            modalMode === "add"
                                                ? "Add Task"
                                                : modalMode === "edit"
                                                    ? "Edit Task"
                                                    : "View Task"
                                        }}
                                    </h2>
                                    <label class="block mb-2">Task Title</label>
                                    <input v-model="taskForm.title" :readonly="modalMode === 'view'"
                                        class="w-full p-2 mb-2 border rounded" type="text" />

                                    <label class="block mb-2">Status</label>
                                    <select v-model="taskForm.status" :disabled="modalMode === 'view'"
                                        class="w-full p-2 mb-4 border rounded">
                                        <option value="Pending">Pending</option>
                                        <option value="Completed">
                                            Completed
                                        </option>
                                    </select>

                                    <label class="block mb-2">Description</label>
                                    <textarea v-model="taskForm.description" :readonly="modalMode === 'view'"
                                        class="w-full p-2 mb-2 border rounded"></textarea>

                                    <div class="flex justify-end space-x-2">
                                        <button @click="closeModal" class="px-4 py-2 text-white bg-gray-500 rounded">
                                            Close
                                        </button>
                                        <button v-if="modalMode !== 'view'" @click="saveTask"
                                            class="px-4 py-2 text-white bg-blue-500 rounded">
                                            Save
                                        </button>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </AuthenticatedLayout>
</template>

<script setup>
import { ref } from "vue";

const tasks = ref([]);
const isModalOpen = ref(false);
const modalMode = ref("add"); // 'add', 'edit', or 'view'
const editingTask = ref(null);
const taskForm = ref({
    title: "",
    description: "",
    status: "Pending",
    createdAt: "",
});

const openModal = (mode, task = null) => {
    modalMode.value = mode;
    isModalOpen.value = true;
    if (task) {
        editingTask.value = task;
        taskForm.value = { ...task };
    } else {
        editingTask.value = null;
        taskForm.value = {
            title: "",
            description: "",
            status: "Pending",
            createdAt: new Date().toLocaleString(),
        };
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
    tasks.value = tasks.value.filter((task) => task.id !== id);
};
</script>
