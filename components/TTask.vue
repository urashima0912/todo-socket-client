<template>
  <div class="t-task">
    <div>
      <img class="width-100" :src="photo" alt="" />
    </div>
    <div>
      <strong>{{ title }}</strong>
      <p>{{ description }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      required: true,
    },
    description: {
      type: String,
      required: true,
    },
    photo: {
      type: String,
      required: true,
    },
  },
}
</script>

<style>
.t-task {
  display: grid;
  grid-template-columns: 10% 90%;
  background-color: #fdcb6e;
}
</style>
