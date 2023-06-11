<template>
  <!-- <h1 v-bind:class="dynamicId">Hello World</h1>
  <h2
    v-for="[taskIndex, task] of Object.entries(tasks)"
    :key="taskIndex"
    :class="even(taskIndex) ? 'text-blue-500' : 'text-green-500'"
  >
    {{ task.title }}
  </h2>
  <input v-model="previousValue" />
  <input v-model="nextValue" />
  <p>{{ hasil }}</p> -->
  <nav class="flex items-end px-4 py-2 space-x-4">
    <h1 class="text-2xl">TaskList</h1>
    <ul>
      <li><router-link to="/">Home</router-link></li>
    </ul>
  </nav>
  <div class="list-task px-4">
    <p v-if="isEmpty">Belum ada task</p>
    <div
      v-for="[taskIndex, task] of Object.entries(tasks)"
      :key="taskIndex"
      class="item-task flex items-start border-b-2 pt-3 pb-4"
    >
      <input
        v-model="task.isDone"
        type="checkbox"
        name="status"
        id="task"
        class="me-2 mt-2"
      />
      <div class="flex flex-col">
        <div :class="{ 'line-through': task.isDone }" class="title-task mb-1">
          {{ task.title }}
        </div>
        <div class="description-task text-sm text-gray-500">
          {{ task.description }}
        </div>
        <a class="btn btn-link p-0" @click="deleteTask(taskIndex)">delete</a>
      </div>
    </div>
    <div class="action py-2">
      <a v-if="!isCreating" href="#" class="btn btn-link" @click="toggleForm"
        >Add Task</a
      >
      <div v-else class="add-card">
        <div class="card mb-2">
          <div
            class="card-body flex flex-col p-0 border border-slate-500 rounded-md"
          >
            <input
              v-model="formData.title"
              class="input border-0 mb-2 focus:outline-none"
              placeholder="Title"
              type="text"
            />
            <textarea
              v-model="formData.description"
              class="input border-0 text-sm focus:outline-none"
              placeholder="Description"
              rows="3"
            ></textarea>
          </div>
        </div>
        <div class="button-wrapper d-flex">
          <button class="btn btn-primary" @click="save">Save</button>
          <button @click="toggleForm" class="btn btn-outline-secondary">
            Cancel
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        tasks: [],
        isCreating: false,
        formData: {
          title: "",
          description: "",
          isDone: false,
        },
      }
    },
    methods: {
      addTask() {
        this.tasks.push(this.formData)
      },
      deleteTask(index) {
        this.tasks.splice(index, 1)
      },
      clearForm() {
        this.formData = {
          title: "",
          description: "",
          isDone: false,
        }
      },
      save() {
        this.addTask()
        this.clearForm()
        this.toggleForm()
      },
      toggleForm() {
        this.isCreating = !this.isCreating
      },
    },
    computed: {
      isEmpty() {
        return this.tasks.length <= 0
      },
    },
  }
</script>
