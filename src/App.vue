<template>
  <header>To do list:</header>
  <ul>
    <input v-model="newTask.text" placeholder="Remind me to..." /><button
      @click="addTask"
    >
      Add task</button
    ><br /><input
      class="urgent-checkbox"
      type="checkbox"
      v-model="isChecked"
    />Urgent
    <li
      v-for="(task, index) in filterTasks"
      :key="index"
      :class="[task.urgent === true ? 'urgent' : '']"
    >
      {{ task.text }}
      <span class="close-btn" @click="removeTask(task)">&#x2716;</span>
    </li>
    <p v-if="emptyList">You have no tasks left to do today!</p>
    <p v-if="!emptyList && emptyUrgentList">
      You currently have no <span style="color: red">urgent</span> tasks to
      handle
    </p>
    <p v-if="!emptyList && emptyNonUrgentList">
      You currently have no non-urgent tasks to handle
    </p>
    <button
      @click="toggleUrgent"
      v-if="!this.showOnlyUrgent && urgentList && nonUrgentList"
      :class="[emptyList ? 'toggleUrgent-btn-inactive' : 'toggleUrgent-btn']"
    >
      Show only urgent tasks
    </button>
    <button
      @click="toggleUrgent"
      v-if="this.showOnlyUrgent"
      :class="[emptyList ? 'toggleUrgent-btn-inactive' : 'toggleUrgent-btn']"
    >
      Show only non-urgent tasks
    </button>

    <br />
    <button
      @click="showAllTasks"
      v-if="this.showOnlyUrgent || this.showOnlyUrgent === false"
      :class="[emptyList ? 'showAll-btn-inactive' : 'showAll-btn']"
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
        return this.tasks.filter((item) => item.urgent);
      } else if (this.showOnlyUrgent === null) {
        return this.tasks;
      } else {
        return this.tasks.filter((item) => !item.urgent);
      }
    },
    emptyList() {
      return this.tasks.length === 0;
    },
    emptyUrgentList() {
      const urgentList = this.tasks.filter((item) => item.urgent);
      return this.showOnlyUrgent && urgentList.length === 0;
    },
    emptyNonUrgentList() {
      const nonurgentList = this.tasks.filter((item) => !item.urgent);
      return this.showOnlyUrgent === false && nonurgentList.length === 0;
    },
    urgentList() {
      const urgentList = this.tasks.filter((item) => item.urgent);
      return urgentList.length >= 1;
    },
    nonUrgentList() {
      const nonUrgentList = this.tasks.filter((item) => !item.urgent);
      return nonUrgentList.length >= 1;
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

.urgent-checkbox {
  margin-top: 10px;
}

.toggleUrgent-btn,
.showAll-btn {
  margin-top: 10px;
}

.showAll-btn-inactive,
.toggleUrgent-btn-inactive {
  display: none;
}

.urgent {
  color: red;
}

.close-btn {
  color: black;
  cursor: pointer;
}
</style>
