<template>
  <header>To do list:</header>
  <ul>
    <input v-model="newTask.text" placeholder="Remind me to..." /><button
      @click="addTask"
    >
      Add task</button
    ><br /><input type="checkbox" v-model="isChecked" />Urgent
    <li
      v-for="(task, index) in tasks"
      :key="index"
      :class="[task.urgent === true ? 'urgent' : '']"
    >
      {{ task.text }}
      <span class="close-btn" @click="removeTask(task)">&#x2716;</span>
    </li>
    <button class="showUrgent-btn" @click="showUrgent">
      Show only urgent tasks
    </button>
    <br />
  </ul>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      tasks: [
        {
          text: "Grocery Shopping",
          urgent: true,
        },
        {
          text: "Take out the trash",
          urgent: true,
        },
        {
          text: "Car wash",
          urgent: false,
        },
      ],
      newTask: {
        text: "",
        urgent: false,
      },
      isChecked: false,
    };
  },
  methods: {
    addTask() {
      if (this.newTask.text) {
        if (this.isChecked) {
          this.newTask.urgent = true;
        }
        this.tasks.push({ ...this.newTask });
        this.newTask.text = "";
        this.isChecked = false;
        this.newTask.urgent = false;
      }
    },
    removeTask(id) {
      this.tasks = this.tasks.filter((element) => element !== id);
    },
    showUrgent() {
      this.tasks = this.tasks.filter((element) => {
        return element.urgent === true;
      });
    },
  },
};
</script>

<style>
* {
  font-family: "Roboto Mono", monospace;

  color: black;
}

body {
  background: #98b4d4;
}

header {
  font-size: 50px;
  text-align: center;
}

ul {
  text-align: center;
  list-style-type: none;
}

ul li {
  padding-top: 10px;
}

button {
  font-weight: bold;
  cursor: pointer;
}

.showUrgent-btn {
  margin-top: 10px;
}

.urgent {
  color: red;
}

.close-btn {
  color: black;
  cursor: pointer;
}
</style>
