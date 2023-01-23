<template>
  <div>
    <div class="wrapper">
      <h1>{{ title }}</h1>
      <form @submit.prevent="addTask">
        <input type="text" required="required" v-model="inputValue" />
        <p>{{ inputValue }}</p>
        <button type="submit">Add New Note</button>
      </form>
      <ul>
        <li v-for="(task, idx) in tasks" :key="task.text">
          {{ idx + 1 }}. {{ task.text }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import { reactive, ref } from "vue";
export default {
  name: "HelloWorld",
  setup() {
    const title = ref("To-Do");
    const inputValue = ref("");
    const tasks = ref([]);
    const state = reactive({
      // double: computed(() => console.log("Test")),
    });
    //Добавить таску
    const addTask = () => {
      tasks.value.push({
        text: inputValue.value.trim(),
        completed: false,
        err: false,
      });
      inputValue.value = "";
    };
    return { state, title, inputValue, tasks, addTask };
  },
};
</script>

<style>
li {
  list-style-type: none;
}

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
