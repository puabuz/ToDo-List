<template>
  <div>
    <div class="wrapper">
      <h1>{{ title }}</h1>
      <form @submit.prevent="addTask">
        <input type="text" required="required" v-model="inputValue" />
        <button type="submit">Add New Note</button>
      </form>
      <span v-if="tasks.length == 0">Создайте свою первую заметку</span>
      <ul v-if="tasks.length !== 0">
        <li v-for="(task, idx) in tasks" :key="task.text">
          {{ idx + 1 }}. {{ task.text }}
          <button @click="deleteTask(task.id)">DELETE</button>
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
        id: tasks.value.length + 1,
        text: inputValue.value.trim(),
        completed: false,
      });
      inputValue.value = "";
    };
    const deleteTask = (taskId) => {
      tasks.value = tasks.value.filter((task) => {
        return task.id !== taskId;
      });
    };
    return { state, title, inputValue, tasks, addTask, deleteTask };
  },
};
</script>

<style>
span {
  display: inline-block;
  margin: 10px 0;
}

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
