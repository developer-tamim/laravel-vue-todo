<template>
    <div>
      <form @submit.prevent="store">
        <div>
          <label for="title">Task Title:</label>
          <input type="text" id="title" v-model="form.title" required />
        </div>
        <div>
          <label for="description">Task Description:</label>
          <textarea id="description" v-model="form.description"></textarea>
        </div>
        <div>
          <label for="status">Task Status:</label>
          <select id="status" v-model="form.status">
            <option value="Pending">Pending</option>
            <option value="Completed">Completed</option>
          </select>
        </div>
        <button type="submit">Create Task</button>
      </form>
    </div>
  </template>

  <script>
  export default {
    data() {
      return {
        form: {
          title: '',
          description: '',
          status: 'Pending',
        },
      };
    },
    methods: {
      store() {
        this.$inertia.post(route('todos.store'), this.form);
      },
    },
  };
  </script>
