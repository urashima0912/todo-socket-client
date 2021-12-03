<template>
  <div class="t-form">
    <img class="width-100" :src="url" alt="image not found" />
    <input id="file" ref="file" type="file" @change="onChange" />
    <input type="text" placeholder="title" v-model="title" />
    <input type="text" placeholder="description" v-model="description" />
    <input type="button" value="submit" @click="onClick" />
  </div>
</template>

<script>
export default {
  props: {
    taskCreated: {
      type: Function,
      required: true,
    },
  },
  data() {
    return {
      url: '',
      title: '',
      description: '',
    }
  },

  methods: {
    onChange() {
      const file = this.$refs.file.files[0]
      console.log('file: ', file)
      const reader = new FileReader()
      reader.onloadend = () => {
        this.url = reader.result
      }
      if (file) {
        reader.readAsDataURL(file)
      }
    },

    async onClick() {
      const body = JSON.stringify({
        title: this.title,
        description: this.description,
        photo: this.url,
      })

      const res = await fetch('http://localhost:3500/api/task/create', {
        method: 'post',
        headers: {
          'Content-Type': 'application/json',
        },
        body,
      })

      const data = await res.json()

      // Begin - Socket
      this.taskCreated(data.task)
      // End - Socket

      console.log({ data })
    },
  },
}
</script>

<style>
.t-form {
  display: grid;
  grid-template-columns: 1fr;
  row-gap: 5px;
  background: #ffeaa7;
  padding: 10px;
}
</style>
