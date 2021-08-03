<template>
  <div :class="[task.status ? 'done' : '', 'task']">
    <div class="control-bar">
      <div>
        <h3>
          <input
            type="text"
            name="text"
            :value="task.title"
            ref="title"
            @change="changes"
          />
        </h3>
        <i @click="showDetail()" class="fas fa-info-circle"> </i>
      </div>
      <div class="icon-display">
        <i @click="$emit('save-task')" class="fas fa-save"> </i>
        <i @click="$emit('delete-task', task.id)" class="fas fa-trash"> </i>
      </div>
    </div>
    <ul v-if="haveDetail" class="task-detail">
      <li>
        Content:
        <input
          type="text"
          name="text"
          :value="task.content"
          ref="content"
          @change="changes"
        />
      </li>
      <li>
        <div class="status-box">
          Done?
          <input
            type="checkbox"
            name="status"
            :checked="task.status"
            @change="changes"
            ref="status"
          />
        </div>
      </li>
      <li>Day created: {{ task.createdDay }}</li>
    </ul>
    <div :key="subTask.id" v-for="subTask in task.subItems" ref="subTask">
      <SubTask
        @sub-content-change="subContentChange"
        @delete-sub-task="$emit('delete-sub-task', [task.id, subTask.id])"
        :subTask="subTask"
      />
    </div>
    <div class="icon-display">
      <i @click="showAddForm()" class="fas fa-plus"> </i>
    </div>
    <AddForm @add-sub-task="addSubTask" v-if="haveAddForm" />
  </div>
</template>

<script>
import SubTask from "../subtask/SubTask";
import AddForm from "../subtask/AddForm";

export default {
  name: "Task",
  props: {
    task: Object,
  },
  methods: {
    subContentChange(data) {
      this.$emit("sub-content-change", [this.task.id, {}, data]);
    },
    showDetail() {
      this.haveDetail = !this.haveDetail;
    },
    saveClick() {
      console.log(this.$refs["subTask"]);
    },
    showAddForm() {
      this.haveAddForm = !this.haveAddForm;
    },
    addSubTask(subTask) {
      this.$emit("add-sub-task", [this.task.id, subTask]);
      this.haveAddForm = !this.haveAddForm;
    },
    changes() {
      this.changeContent = true;
      if (this.haveDetail) {
        this.$emit("content-change", [
          this.task.id,
          {
            title: this.$refs["title"].value,
            content: this.$refs["content"].value,
            status: this.$refs["status"].checked,
          },
        ]);
      } else {
        this.$emit("content-change", [
          this.task.id,
          {
            title: this.$refs["title"].value,
          },
        ]);
      }
    },
  },
  components: {
    SubTask,
    AddForm,
  },
  data() {
    return {
      haveDetail: false,
      haveAddForm: false,
      changeContent: false,
    };
  },
  emits: [
    "delete-sub-task",
    "add-sub-task",
    "delete-task",
    "sub-content-change",
    "content-change",
    "save-task",
  ],
};
</script>

<style scope>
.fas {
  color: rgb(33, 182, 120);
}
.fa-trash {
  color: rgb(230, 71, 31);
}
.fa-save {
  color: rgb(177, 29, 127);
}
.task {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}

.task.done h3 {
  text-decoration-line: line-through;
}
.task h3 {
  display: flex;
  align-items: center;
  justify-content: space-between;
  color: green;
}

.sub-task {
  display: flex;
  flex-direction: column;
  align-items: left;
  justify-content: space-between;
}
.sub-task.done {
  text-decoration-line: line-through;
}

.control-bar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin: 5px;
}

.icon-display {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin: 5px;
  width: 40px;
}

.sub-task-detail {
  border: solid 1px;
  display: flex;
  flex-direction: column;
  height: 200px;
}
.task-detail {
  border: solid 1px;
  display: flex;
  flex-direction: column;
}
.task-detail input {
  width: 75%;
  margin: 5px;
  padding: 3px 7px;
  font-size: 17px;
  background: transparent;
  border: none;
  color: blue;
}
li {
  display: block;
  height: 30px;
  margin: 0 auto;
  width: 95%;
}
.add-form {
  margin-bottom: 40px;
}
.form-control {
  margin: 20px 0;
}
.form-control label {
  display: block;
}
.form-control input {
  width: 100%;
  height: 40px;
  margin: 5px;
  padding: 3px 7px;
  font-size: 17px;
}
.form-control-check {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.form-control-check label {
  flex: 1;
}
.form-control-check input {
  flex: 2;
  height: 20px;
}
h3 input {
  width: 80%;
  font-size: 20px;
  background: transparent;
  border: none;
  color: rgb(28, 170, 63);
}
h3 {
  width: 160%;
}
</style>
