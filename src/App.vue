<template>
  <div class="container">
    <Header
      @show-add-form="showAddForm"
      title="Todo list"
      :haveAddForm="haveAddForm"
    />
    <AddForm @add-task="addTask" v-if="haveAddForm" />
    <Tasks
      @save-task="saveTask"
      @content-change="contentChange"
      @sub-content-change="subContentChange"
      @delete-sub-task="deleteSubTask"
      @add-sub-task="addSubTask"
      @delete-task="deleteTask"
      :tasks="tasks"
    />
  </div>
</template>

<script>
import Header from "./components/Header";
import Tasks from "./components/task/Tasks";
import AddForm from "./components/task/AddForm";

export default {
  name: "App",
  components: {
    Header,
    Tasks,
    AddForm,
  },
  data() {
    return {
      tasks: [],
      haveAddForm: false,
      listeningTasks: [],
      listeningSubTasks: [],
    };
  },
  async created() {
    this.tasks =
      window.localStorage.getItem("data") == null
        ? []
        : JSON.parse(window.localStorage.getItem("data"));
  },
  methods: {
    showAddForm() {
      this.haveAddForm = !this.haveAddForm;
    },
    update() {
      window.localStorage.setItem("data", JSON.stringify(this.tasks));
    },
    subContentChange(data) {
      //data[taskid, [task content], [subtask content]]
      if (this.listeningSubTasks.length == 0) {
        this.listeningSubTasks = [...this.listeningSubTasks, data];
      } else {
        let addFlag = true;
        this.listeningSubTasks.forEach((element) => {
          if (element[0] === data[0] && data[2][0] === element[2][0]) {
            addFlag = false;
            element[2] = data[2];
          }
        });
        if (addFlag) {
          this.listeningSubTasks = [...this.listeningSubTasks, data];
        }
      }
      console.log(this.listeningSubTasks);
      this.update();
    },
    contentChange(data) {
      if (this.listeningTasks.length == 0) {
        this.listeningTasks = [...this.listeningTasks, [data[0], data[1]]];
      } else {
        let addFlag = true;
        this.listeningTasks.forEach((element) => {
          if (element[0] === data[0]) {
            addFlag = false;
            element[1] = data[1];
          }
        });
        if (addFlag) {
          this.listeningTasks = [...this.listeningTasks, [data[0], data[1]]];
        }
      }
      console.log(this.listeningTasks);
      this.update();
    },
    addTask(task) {
      this.tasks = [...this.tasks, task];
      this.haveAddForm = !this.haveAddForm;
      this.update();
    },
    deleteTask(id) {
      this.tasks = this.tasks.filter((task) => task.id !== id);
      this.update();
    },
    deleteSubTask(listId) {
      console.log(listId);
      this.tasks = this.tasks.map((task) =>
        task.id === listId[0]
          ? {
              ...task,
              subItems: task.subItems.filter(
                (subTask) => subTask.id !== listId[1]
              ),
            }
          : task
      );
      this.update();
    },
    addSubTask(info) {
      console.log(info);
      this.tasks = this.tasks.map((task) =>
        task.id === info[0]
          ? {
              ...task,
              subItems: [...task.subItems, info[1]],
            }
          : task
      );
      this.update();
    },
    saveTask() {
      this.tasks.forEach((task) => {
        this.listeningTasks.forEach((ele) => {
          if (ele[0] === task.id) {
            task.title = ele[1].title;
            task.content = ele[1].content;
            task.status = ele[1].status;
          }
        });
      });
      this.tasks.forEach((task) => {
        this.listeningSubTasks.forEach((ele) => {
          if (ele[0] === task.id) {
            task.subItems.forEach((sub) => {
              if (sub.id === ele[2][0]) {
                sub.title = ele[2][1].title;
                sub.content = ele[2][1].content;
                sub.status = ele[2][1].status;
                sub.note = ele[2][1].note;
              }
            });
          }
        });
      });
      console.log(this.tasks);
      this.update();
    },
  },
  emits: [
    "delete-task",
    "add-task",
    "delete-sub-task",
    "add-sub-task",
    "content-change",
    "save-task",
  ],
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap");
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: "Poppins", sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
.btn-detail {
  background: rgb(219, 97, 138);
  padding: 5px 10px;
}
</style>
