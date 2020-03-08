<template>
  <div class="container">
    <header>
      <h1>Todo List</h1>
    </header>
    <form className="new-task" @submit.prevent="handleSubmit">
      <input
        type="text"
        placeholder="Type to add new tasks"
        v-model="newTask"
      />
    </form>
    <ul>
      <Task v-for="task in tasks" v-bind:key="task._id" v-bind:task="task" />
    </ul>
  </div>
</template>

<script>
import Vue from "vue";
import Task from "./Task.vue";
import { Tasks } from "../api/tasks";

export default {
  components: {
    Task
  },
  data() {
    return {
      newTask: ""
    };
  },
  methods: {
    getTasks() {
      return {};
    },
    handleSubmit() {
      Tasks.insert({
        text: this.newTask,
        createdAt: new Date()
      });

      this.newTask = "";
    }
  },
  meteor: {
    tasks() {
      return Tasks.find({}, { sort: { createdAt: -1 } }).fetch();
    }
  }
};
</script>
