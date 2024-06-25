<template>
  <div :class="['message', message.role]">
    <p>{{ message.content }}</p>
  </div>
</template>

<script>
export default {
  props: {
    message: {
      type: Object,
      required: true
    }
  }
};
</script>

<style scoped>
.message {
  margin: 10px 0;
  padding: 10px;
  border-radius: 5px;
}
.message.agent {
  background-color: #d3d3d3;
  text-align: left;
}
.message.user {
  background-color: #42b983;
  color: white;
  text-align: right;
}
</style>
