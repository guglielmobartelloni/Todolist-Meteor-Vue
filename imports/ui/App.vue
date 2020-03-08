<template>
  <div class="container">
    <header>
      <h1>Todo List ({{ incompleteCount() }})</h1>

      <label className="hide-completed">
        <input
          type="checkbox"
          readOnly
          checked="hideCompleted"
          v-model="hideCompleted"
          @click="toggleHideCompleted"
        />
        Hide Completed Tasks
      </label>
    </header>
    <blaze-template template="loginButtons" tag="span"></blaze-template>

    <template v-if="currentUser">
      <form class="new-task" @submit.prevent="handleSubmit">
        <input
          type="text"
          placeholder="Type to add new tasks"
          v-model="newTask"
        />
      </form>
    </template>

    <ul>
      <Task v-for="task in tasks" v-bind:key="task._id" v-bind:task="task" />
    </ul>
  </div>
</template>

<script>
import Vue from "vue";
import Task from "./Task.vue";
import { Tasks } from "../api/tasks";
import { Meteor } from "meteor/meteor";

export default {
  components: {
    Task
  },
  data() {
    return {
      newTask: "",
      hideCompleted: false
    };
  },
  methods: {
    getTasks() {
      return {};
    },
    handleSubmit() {
      Tasks.insert({
        text: this.newTask,
        createdAt: new Date(),
        owner: Meteor.userId(), // _id of logged in user
        username: Meteor.user().username // username of logged in user
      });

      this.newTask = "";
    },
    toggleHideCompleted() {
      this.hideCompleted = !this.hideCompleted;
    },
    incompleteCount() {
      return Tasks.find({ checked: { $ne: true } }).count();
    },
    currentUser() {
      return Meteor.user();
    }
  },
  meteor: {
    tasks() {
      let filteredTasks = Tasks.find({}, { sort: { createdAt: -1 } }).fetch();
      if (this.hideCompleted) {
        filteredTasks = filteredTasks.filter(task => !task.checked);
      }
      return filteredTasks;
    }
  }
};
</script>
