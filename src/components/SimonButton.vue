<template>
  <button class="simon-btn" :class="color"></button>
</template>
<script>
export default {
  name: "simon-button",
  props: {
    color: {
      type: String,
      required: true,
    },
  },
};
</script>
<style scoped>
.simon-btn {
  width: 20vh;
  height: 20vh;
  display: inline-block;
  margin: 2px;
  border: 2px solid black;
}

.red {
  background-color: red;
  border-top-right-radius: 100%;
}
.green {
  background-color: green;
  border-top-left-radius: 100%;
}
.blue {
  background-color: blue;
  border-bottom-right-radius: 100%;
}
.yellow {
  background-color: yellow;
  border-bottom-left-radius: 100%;
}
.active {
  background-color: white;
}
</style>
