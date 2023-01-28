<template>
  <div>
    <div class="wrapper">
      <h1>{{ title }}</h1>
      <form @submit.prevent="addTask">
        <input type="text" required="required" v-model="inputValue" />
        <button type="submit">Add New Note</button>
      </form>
      <span v-if="tasks.length == 0">Создайте свою первую заметку</span>
      <ul>
        <li v-for="(task, idx) in tasks" :key="task.id">
          {{ idx + 1 }}. {{ task.text }}
          <input
            type="text"
            v-if="task.editing"
            v-model="editValue"
            :key="task.id"
          />
          <button @click="deleteTask(task.id)">DELETE</button>
          <button v-if="!task.editing" @click="editTask(task.text)">
            EDIT
          </button>
          <button v-else @click="saveEdit(task.text)">Save</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import { reactive, ref, onMounted, watch } from "vue";
export default {
  name: "HelloWorld",
  setup() {
    const title = ref("To-Do");
    const inputValue = ref("");
    const editValue = ref("");
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
        editing: false,
      });
      inputValue.value = "";
    };
    //Удалить таску
    const deleteTask = (taskId) => {
      tasks.value = tasks.value.filter((task) => {
        return task.id !== taskId;
      });
    };
    //Редактировать таску
    const editTask = (taskText) => {
      editValue.value = taskText;
      console.log(taskText);
      tasks.value = tasks.value.map((task) => {
        if (task.text === taskText) {
          task.editing = !task.editing;
        }
        return task;
      });
    };
    //Сохранить отредактированную таску
    const saveEdit = (taskText) => {
      if (editValue.value == "") {
        editValue.value = "Введите текст...";
        setTimeout(() => {
          editValue.value = taskText;
        }, 2000);
      } else {
        tasks.value = tasks.value.map((task) => {
          if (task.text === taskText) {
            task.text = editValue.value.trim();
            task.editing = !task.editing;
          }
          return task;
        });
      }
    };
    onMounted(() => {
      console.log("Mount");
      const localNotes = localStorage.getItem("tasks");
      if (localNotes) tasks.value = JSON.parse(localNotes);
    });
    watch(
      tasks,
      (task) => {
        localStorage.setItem("tasks", JSON.stringify(task));
      },
      {
        deep: true,
      }
    );
    return {
      state,
      title,
      inputValue,
      editValue,
      tasks,
      addTask,
      deleteTask,
      editTask,
      saveEdit,
    };
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
