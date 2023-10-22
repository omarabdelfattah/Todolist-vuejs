<template>
  <div class="container">
    <div class="task">
      <!-- title -->
      <div class="title">
        <h1>To Do List</h1>
      </div>
      <!-- form -->
      <div class="form">
        <input type="text" placeholder="New Task" v-model="newTask" @keyup.enter="addTask" />
        <button @click="addTask"><i class="fas fa-plus"></i></button>
      </div>
      <!-- task lists -->
      <div class="taskItems">
        <ul>
          <TaskItem v-bind:task="task"  
          v-for="(task, index) in tasks" 
          :key="task.id"
          @remove="removeTask(index)"
          @toggle="toggleTask(task)"
          ></TaskItem>
        </ul>
      </div>
      <!-- buttons -->
      <div class="clearBtns">
        <button @click="clearCompleted">Clear completed</button>
        <button @click="clearAll">Clear all</button>
      </div>
      <!-- pending task -->
      <div class="pendingTasks">
        <span>Pending Tasks: {{ inComplete }}</span>
      </div>
    </div>
  </div>
</template>

<script>
import TaskItem from './Task-item.vue';

export default {
  name: "Task",
  props: ["tasks"],
  components: {
    TaskItem
  },
  data() {
    return {
      newTask: ""
    }
  },
  computed: {
    inComplete(){
      return this.tasks.filter((task)=> this.inProgress(task)).length;
    }
  },
  methods: {
    addTask(){
      if(this.newTask){
        this.tasks.push({
          title: this.newTask,
          completed: false
        });
        this.newTask = "";
      }
    },
    inProgress(task){
      return !this.isCompleted(task);
    },
    isCompleted(task){
      return task.completed;
    },
    clearCompleted(){
      this.tasks = this.tasks.filter((task)=> this.inProgress(task));
    },
    clearAll(){
      this.tasks = [];
    },
    removeTask(index){
      this.tasks.splice(index,1);
    },
    toggleTask(task){
      task.completed = !task.completed;
    }
  }
};
</script>
