<template>
  <div id="app">
    <h1>Tasks</h1>

    <tasks-form @Adicionar_nova_tarefa="addTask" />
    <tasks-table
      v-bind:tasks="tasks"
      @Empty_List="deleteList"
      @Clear_Tasks="clearTasks"
      @Save="persist"
    />
  </div>
</template>

<script>
import TasksTable from "@/components/TasksTable.vue";
import TasksForm from "@/components/TasksForm.vue";

export default {
  name: "App",
  components: {
    TasksTable,
    TasksForm,
  },
  data() {
    return {
      tasks: [],
    };
  },
  mounted() {
    if (localStorage.getItem("tasks")) {
      try {
        this.tasks = JSON.parse(localStorage.getItem("tasks"));
      } catch (e) {
        localStorage.removeItem("tasks");
      }
    }
  },
  methods: {
    persist() {
      const parsed = JSON.stringify(this.tasks);
      localStorage.setItem("tasks", parsed);
    },
    addTask(task) {
      const id =
        this.tasks.length > 0 ? this.tasks[this.tasks.length - 1].id + 1 : 0;
      const New_task = { ...task, id };
      this.tasks = [...this.tasks, New_task];
    },
    deleteList() {
      this.tasks = [];
    },
    clearTasks(clearlist) {
      for (var i = 0; i < clearlist.length; i++) {
        var holder = this.tasks[clearlist[i] - 1];
        this.tasks[clearlist[i] - 1] = this.tasks[i];
        this.tasks[i] = holder;
      }
      this.tasks.splice(0, clearlist.length);
      for (var j = 0; j < this.tasks.length; j++) {
        this.tasks[j].id = j + 1;
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
