<template>
  <div>
    <div class="wrapper">
      <h1>{{ title }}</h1>
      <form @submit.prevent="AddTask">
        <input type="text" />
        <button type="submit">Add New Note</button>
      </form>
    </div>
  </div>
</template>

<script>
import { computed, reactive, ref } from "vue";
export default {
  name: "HelloWorld",
  setup() {
    const title = ref("To-Do");
    const state = reactive({
      count: 0,
      double: computed(() => console.log("Test")),
    });
    return { state, title };
  },
};
</script>

<style>
.one {
  border: 1px solid black;
  width: 50px;
  height: 50px;
  margin: 0 auto;
  background-color: rgb(230, 221, 221);
  cursor: pointer;
}

button {
  margin: 0 5px;
}
</style>
