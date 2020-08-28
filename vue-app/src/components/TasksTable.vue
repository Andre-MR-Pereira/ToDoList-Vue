<template>
  <div id="Tasks-table">
    <p v-if="tasks.length < 1" class="empty-table">No Tasks</p>
    <table v-else>
      <thead>
        <tr>
          <th>Task</th>
          <th>Due</th>
          <th>Completed</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="task in tasks" :key="task.id">
          <td>{{task.name}}</td>
          <td>{{task.due}}</td>
          <td>
            <label class="container">
              <input type="checkbox" :id="task.id" @click="markTask(task)" />
              <span class="checkmark"></span>
            </label>
          </td>
        </tr>
      </tbody>
    </table>
    <div class="function-buttons">
      <button style="margin: 5px;" @click="clearPush">Clear Completed</button>
      <button style="margin: 5px;" class="empty-button" @click="emptyList">Empty List</button>
      <button style="margin: 5px;" class="save-button">Save</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "tasks-table",
  props: {
    tasks: Array,
  },
  data() {
    return {
      clearlist: [],
    };
  },
  methods: {
    emptyList() {
      this.$emit("Empty_List");
    },
    clearPush() {
      for (var i = 0; i < this.clearlist.length; i++) {
        document.getElementById(this.clearlist[i]).checked = false;
      }
      this.$emit("Clear_Tasks", this.clearlist);
      this.clearlist = [];
    },
    markTask(task) {
      var checkBox = document.getElementById(task.id);

      if (checkBox.checked == true) {
        this.clearlist = [...this.clearlist, task.id];
      } else {
        for (var i = 0; i < this.clearlist.length; i++) {
          if (this.clearlist[i] === task.id) {
            this.clearlist.splice(i, 1);
          }
        }
      }
    },
  },
};
</script>

<style scoped>
.function-buttons {
  text-align: center;
}
.save-button {
  background-color: green;
  margin: 5px;
}
.save-button:hover {
  background-color: rgb(1, 160, 1);
  margin: 5px;
}
.empty-button {
  background-color: red;
  margin: 5px;
}
.empty-button:hover {
  background-color: rgb(173, 0, 0);
  margin: 5px;
}
</style>