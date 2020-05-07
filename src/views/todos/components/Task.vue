<template>
  <div class="todo" :style="{ borderColor: statusColor(task.status) }">
    <input
      :value="value"
      @input="handleInput($event.target.value)"
      @blur="editOff"
      :readonly="readOnly"
      ref="inputTodoText"
      spellcheck="false"
      :class="inputClass"
    />
    <div class="todo-buttons">
      <div
        class="todo-button todo-button-edit"
        @click="editOn"
        title="edit task"
      ></div>
      <div
        class="todo-button todo-button-complete"
        @click="
          $emit('todoChangeStatus', {
            id: task.id,
            status: STATUS.TODO_DONE,
          })
        "
      ></div>
      <div
        class="todo-button todo-button-delete"
        @click="$emit('deleteTask', task.id)"
      ></div>
    </div>
  </div>
</template>

<script>
import STATUS from "../constants";

export default {
  model: {
    prop: "value",
    event: "input",
  },
  /* -------------------------------------------------------------------------- */

  props: {
    value: String,
    task: Object,
  },
  /* -------------------------------------------------------------------------- */

  data() {
    return {
      readOnly: true,
      STATUS,
    };
  },
  /* -------------------------------------------------------------------------- */

  methods: {
    handleInput(value) {
      this.$emit("input", value);
    },
    statusColor(status) {
      switch (status) {
        case STATUS.TODO_TODO:
          return "#ffff00";
        case STATUS.TODO_DONE:
          return "#60f46b";
        case STATUS.TODO_DELETED:
          return "#f43f11";
      }
    },
    editOn() {
      this.readOnly = false;
      this.$refs.inputTodoText.focus();
    },
    editOff() {
      this.readOnly = true;
    },
  },
  /* -------------------------------------------------------------------------- */

  computed: {
    inputClass() {
      return {
        "todo-text": true,
        editing: !this.readOnly,
      };
    },
  },
};
/* -------------------------------------------------------------------------- */
</script>

<style>
.todo {
  display: flex;
  border-left: 12px solid #ffff00;
  margin: 1.2rem 0;
  border-top-left-radius: 5px;
  border-bottom-left-radius: 5px;
}

.todo-text {
  box-sizing: border-box;
  border: 1px solid transparent;
  background-color: transparent;
  font-size: 1rem;
  color: var(--input-color);
  flex: 1;
  text-align: left;
  margin: 0 1.5rem;
}

.todo-text:focus {
  outline: none;
}

.editing {
  border-bottom: 1px solid grey;
}

.todo-buttons {
  display: flex;
  width: 100px;
  margin-left: 0 1.5rem;
}

.todo-button {
  width: 16px;
  height: 16px;
  border-radius: 100%;
  margin: 0 5px;
  cursor: pointer;
}

.todo-button-complete {
  background-color: #60f46b;
}
.todo-button-edit {
  box-sizing: border-box;
  background-color: transparent;
  border: 2px solid grey;
}

.todo-button-complete {
  background-color: #60f46b;
}

.todo-button-delete {
  background-color: #f43f11;
}
</style>
