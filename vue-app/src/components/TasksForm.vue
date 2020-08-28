<template>
  <div id="task-form">
    <form v-on:submit.prevent="handleSubmit">
      <label>Task</label>
      <input
        ref="first"
        type="text"
        v-model="task.name"
        :class="{'has-error':submitting && invalidName}"
        @focus="clearStatus"
        @keypress="clearStatus"
      />
      <label>Due</label>
      <input
        type="text"
        v-model="task.due"
        :class="{'has-error':submitting && invalidDue}"
        @focus="clearStatus"
        @keypress="clearStatus"
      />
      <p v-if="error && submitting" class="error-message">❗Please fill out all required fields</p>
      <p v-if="success" class="success-message">✅ Task successfully added</p>
      <button>Add Task</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "task-form",
  data() {
    return {
      submitting: false,
      error: false,
      success: false,
      task: {
        name: "",
        due: "",
      },
    };
  },
  methods: {
    handleSubmit() {
      this.submitting = true;
      this.clearStatus();

      if (this.invalidName || this.invalidDue) {
        this.error = true;
        return;
      }
      this.$emit("Adicionar_nova_tarefa", this.task);
      this.$refs.first.focus();
      this.task = {
        name: "",
        due: "",
      };
      this.error = false;
      this.success = true;
      this.submitting = false;
    },

    clearStatus() {
      this.error = false;
      this.success = false;
    },
  },
  computed: {
    invalidName() {
      return this.task.name === "";
    },
    invalidDue() {
      return this.task.due === "";
    },
  },
};
</script>

<style scoped>
form {
  margin-bottom: 2rem;
}
button {
  background-color: white;
  color: black;
  border: 2px solid green;
}
button:hover {
  background-color: green;
  color: white;
}
[class*="-message"] {
  font-weight: 500;
}

.error-message {
  color: #d33c40;
}

.success-message {
  color: #32a95d;
}
</style>