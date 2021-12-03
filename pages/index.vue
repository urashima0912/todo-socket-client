<template>
  <div class="index-page">
    <div class="container">
      <div class="form-container">
        <TForm :taskCreated="taskCreated" />
      </div>
      <div class="list-container">
        <TTask
          class="my-4"
          v-for="task in tasks"
          :key="task.id"
          :title="task.title"
          :description="task.description"
          :photo="task.photo"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  head: {
    script: [
      {
        src: 'http://localhost:3500/socket.io/socket.io.js',
      },
    ],
  },

  data() {
    return {
      tasks: [],
      socket: undefined,
    }
  },
  async beforeMount() {
    await this.updateTasks()
  },

  mounted() {
    this.socket = io('http://localhost:3500')
    console.log({ socket: this.socket })
    this.socket.on('task:update', async (msg) => {
      await this.updateTasks()
    })
  },

  methods: {
    taskCreated(task) {
      this.socket.emit('task:created', { taskId: task._id })
    },

    async updateTasks() {
      const res = await fetch('http://localhost:3500/api/task/all')
      const data = await res.json()
      console.log({ data })
      this.tasks = data.tasks
    },
  },
}
</script>

<style>
.container {
  display: grid;
  grid-template-columns: 30% 70%;
}
</style>
