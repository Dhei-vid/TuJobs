<!-- METHOD 1 -->
<script>
export default {
  data() {
    return {
      name: "David Etuk",
      status: "pending",
      tasks: ["Task1", "Task2", "Task3"],
      link: "https://google.com",
    };
  },
  methods: {
    toggleStatus() {
      if (this.status === "active") {
        this.status = "pending";
      } else if (this.status === "pending") {
        this.status = "inactive";
      } else {
        this.status = "pending";
      }
    },
  },
};
</script>

<template>
  <h1>{{ name }}</h1>
  <p v-if="status === 'active'">User is active</p>
  <p v-if="status === 'pending'">User is pending</p>
  <p v-else>User is inactive</p>

  <h3>Tasks</h3>
  <ul>
    <li v-for="task in tasks" :key="task">{{ task }}</li>
  </ul>

  <!-- <a v-bind:href="link">Click for Google</a> -->
  <a :href="link">Click for Google</a>
  <!-- the column in front of href makes it dynamic -->

  <!-- <button v-on:click="toggleStatus">Change Status</button> -->

  <button @click="toggleStatus">Change Status</button>
</template>

<style scoped>
h1 {
  color: #42b983;
}
</style>

<!-- METHOD 2 -->
<!-- 
<script setup>
import { ref, onMounted } from "vue"; -->

// using a composition API
const name = ref("David Etuk");
const status = ref("pending");
const tasks = ref(["Task1", "Task2", "Task3"]);
const link = ref("https://google.com");
const newTask = ref("");

const toggleStatus = () => {
  if (status.value === "active") {
    status.value = "pending";
  } else if (status.value === "pending") {
    status.value = "inactive";
  } else {
    status.value = "pending";
  }
};

const addTask = () => {
  if (newTask.value.trim() !== "") {
    tasks.value.push(newTask.value);
    newTask.value = "";
  }
};

const removeTask = () => {
  if (tasks.length === 0) return;

  tasks.value.pop();
};

onMounted(async () => {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/todos");
    const data = await response.json();

    tasks.value = data.map((task) => task.title);
  } catch (error) {
    console.log("Error fetching tasks", error);
  }
});
</script>

<template>
  <h1>{{ name }}</h1>
  <p v-if="status === 'active'">User is active</p>
  <p v-if="status === 'pending'">User is pending</p>
  <p v-else>User is inactive</p>

  <h3>Form</h3>
  <form @submit.prevent="addTask">
    <label for="newTask">Add Task</label>
    <input type="text" id="nextTask" name="newTask" v-model="newTask" />

    <button type="submit">Submit</button>
  </form>

  <h3>Tasks</h3>
  <ul>
    <li v-for="task in tasks" :key="task">{{ task }}</li>
  </ul>

  <button @click="removeTask">Remove task</button>

  <!-- <a v-bind:href="link">Click for Google</a> -->
  <a :href="link">Click for Google</a>
  <!-- the column in front of href makes it dynamic -->

  <!-- <button v-on:click="toggleStatus">Change Status</button> -->

  <button @click="toggleStatus">Change Status</button>
</template>

<style scoped>
h1 {
  color: #42b983;
}
</style>
