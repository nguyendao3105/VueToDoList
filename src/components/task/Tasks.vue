<template>
  <div>
    <div :key="task.id" v-for="task in tasks">
      <Task
        @save-task="$emit('save-task')"
        @content-change="contentChange"
        @sub-content-change="subContentChange"
        @delete-sub-task="deleteSubTask"
        @add-sub-task="addSubTask"
        @delete-task="$emit('delete-task', task.id)"
        :task="task"
      />
    </div>
  </div>
</template>

<script>
import Task from "./Task";

export default {
  name: "Tasks",
  props: {
    tasks: Array,
  },
  components: {
    Task,
  },
  methods: {
    onDelete(id) {
      this.$emit("delete-task", id);
    },
    subContentChange(data) {
      this.$emit("sub-content-change", data);
    },
    contentChange(data) {
      this.$emit("content-change", data);
    },
    deleteSubTask(listId) {
      this.$emit("delete-sub-task", listId);
    },
    addSubTask(info) {
      this.$emit("add-sub-task", info);
    },
  },
  emits: [
    "delete-task",
    "delete-sub-task",
    "add-sub-task",
    "sub-content-change",
    "content-change",
    "save-task",
  ],
};
</script>
