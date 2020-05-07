<template>
  <div class="container">
    <div class="content">
      <h3>Tasks:</h3>
      <transition-group
        tag="div"
        enter-active-class="animated fadeInDown"
        leave-active-class="animated fadeOut"
      >
        <Task
          v-for="task in filteredTasks"
          v-model="task.taskDescription"
          @deleteTask="deleteTask"
          @todoChangeStatus="todoChangeStatus"
          :task="task"
          :key="task.id"
        />
      </transition-group>
      <div class="todo-footer">
        <input
          placeholder="Enter a task"
          spellcheck="false"
          class="todo-input"
          type="text"
          v-model="taskDescription"
          @keyup.enter="addTask"
        />
        <button
          :class="[
            'add-task',
            { 'selected-task': filterKey === STATUS.TODO_DONE },
          ]"
          @click="changeFilter(STATUS.TODO_DONE)"
        >
          completed
        </button>
        <button
          :class="[
            'add-task',
            { 'selected-task': filterKey === STATUS.TODO_ALL },
          ]"
          @click="changeFilter(STATUS.TODO_ALL)"
        >
          all
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import Task from "./components/Task.vue";
import STATUS from "./constants";

export default {
  name: "Todo",
  components: {
    Task,
  },
  /* -------------------------------------------------------------------------- */

  data() {
    return {
      taskDescription: "",
      tasks: [],
      filterKey: STATUS.TODO_ALL,
      id: 0,
      STATUS,
    };
  },
  /* -------------------------------------------------------------------------- */

  methods: {
    addTask() {
      if (this.taskDescription === "") return;
      this.tasks.push({
        id: this.id + 1,
        taskDescription: this.taskDescription,
        status: STATUS.TODO_TODO,
      });
      this.id = this.id + 1;
      this.taskDescription = "";
    },
    todoChangeStatus({ id, status }) {
      this.tasks.forEach((task, index) => {
        if (task.id === id) {
          this.tasks[index].status = status;
        }
      });
    },
    deleteTask(id) {
      this.tasks = this.tasks.filter(task => task.id !== id);
    },
    changeFilter(key) {
      this.filterKey = key;
    },
  },
  /* -------------------------------------------------------------------------- */

  computed: {
    filteredTasks() {
      if (this.filterKey === STATUS.TODO_ALL) return this.tasks;
      return this.tasks.filter(task => task.status === this.filterKey);
    },
  },
};
/* -------------------------------------------------------------------------- */
</script>

<style scoped>
.container {
  display: flex;
  align-items: center;
  justify-content: center;
  height: calc(100vh - 100px);
}

.content {
  color: grey;
  width: 80%;
  min-height: 5%;
  background: #171717;
  border-radius: 15px;
  padding: 20px 20px;
  -webkit-box-shadow: 0px 9px 26px -10px rgba(0, 0, 0, 0.49);
  -moz-box-shadow: 0px 9px 26px -10px rgba(0, 0, 0, 0.49);
  box-shadow: 0px 9px 26px -10px rgba(0, 0, 0, 0.49);
}

.todo-footer {
  display: flex;
  padding: 20px;
}

.todo-input {
  flex: 1;
  width: 100%;
  line-height: 25px;
  font-size: 14px;
  font-weight: 500;
  font-family: inherit;
  border-radius: 6px;
  font-size: 1rem;
  color: var(--input-color);
  border: 1px solid grey;
  background: transparent;
  padding: 8px 16px;
  margin-right: 20px;
}

.todo-input:focus {
  outline: none;
  border-color: greenyellow;
}

.add-task {
  color: grey;
  border: solid 1px grey;
  border-radius: 4px;
  display: inline-block;
  background-color: transparent;
  text-align: center;
  text-decoration: none;
  border-radius: 5px;
  min-width: 5rem;
  margin: 0 8px;
}

.selected-task {
  border-color: #60f46b;
}

.add-task:focus {
  outline: none;
  border-color: greenyellow;
}
</style>
