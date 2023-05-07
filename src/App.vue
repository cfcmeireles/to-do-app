<template>
  <header>To do list:</header>
  <ul>
    <input v-model="newTask.text" placeholder="Remind me to..." /><button
      @click="addTask"
    >
      Add task</button
    ><br /><input type="checkbox" v-model="isChecked" />Urgent
    <li
      v-for="(task, index) in filterTasks"
      :key="index"
      :class="[task.urgent === true ? 'urgent' : '']"
    >
      {{ task.text }}
      <span class="close-btn" @click="removeTask(task)">&#x2716;</span>
    </li>
    <p v-show="emptyList">You have no tasks left to do today!</p>
    <p v-show="!emptyList && emptyUrgentList">
      You currently have no <span style="color: red">urgent</span> tasks to
      handle
    </p>
    <p v-show="!emptyList && emptyNonUrgentList">
      You currently have no non-urgent tasks to handle
    </p>
    <button
      class="showUrgent-btn"
      @click="toggleUrgent"
      v-if="this.showOnlyUrgent"
    >
      Show only non-urgent tasks
    </button>
    <button class="showUrgent-btn" @click="toggleUrgent" v-else>
      Show only urgent tasks
    </button>
    <br />
    <button
      class="showAll-btn"
      @click="showAllTasks"
      v-show="this.showOnlyUrgent || this.showOnlyUrgent === false"
    >
      Show all tasks
    </button>
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
      showOnlyUrgent: null,
    };
  },
  computed: {
    filterTasks() {
      if (this.showOnlyUrgent) {
        return this.tasks.filter((item) => item.urgent === true);
      } else if (this.showOnlyUrgent === null) {
        return this.tasks;
      } else {
        return this.tasks.filter((item) => item.urgent === false);
      }
    },
    emptyList() {
      return this.tasks.length === 0 ? true : false;
    },
    emptyUrgentList() {
      const urgentList = this.tasks.filter((item) => item.urgent === true);
      return this.showOnlyUrgent && urgentList.length === 0 ? true : false;
    },
    emptyNonUrgentList() {
      const nonurgentList = this.tasks.filter((item) => item.urgent === false);
      return this.showOnlyUrgent === false && nonurgentList.length === 0
        ? true
        : false;
    },
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
    removeTask(task) {
      this.tasks = this.tasks.filter((element) => element !== task);
    },
    toggleUrgent() {
      this.showOnlyUrgent = !this.showOnlyUrgent;
    },
    showAllTasks() {
      this.showOnlyUrgent = null;
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

.showUrgent-btn,
.showAll-btn {
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
