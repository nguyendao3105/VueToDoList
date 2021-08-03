<template>
  <div :class="[subTask.status ? 'done' : '', 'sub-task']">
    <div class="control-bar">
      <input
        type="text"
        name="text"
        :value="subTask.title"
        ref="subTitle"
        @change="changes"
      />
      <i @click="showDetail()" class="fas fa-info-circle"> </i>

      <div class="icon-display">
        <i @click="$emit('delete-sub-task', subTask.id)" class="fas fa-trash">
        </i>
      </div>
    </div>
    <!-- <SubTaskDetail v-if="this.haveDetail" :subTask="subTask" /> -->
    <ul v-if="haveDetail" class="sub-task-detail">
      <li>
        Content:
        <input
          type="text"
          name="text"
          :value="subTask.content"
          ref="subContent"
          @change="changes"
        />
      </li>
      <li>
        <div class="status-box">
          Done?
          <input
            type="checkbox"
            name="subStatus"
            :checked="subTask.status"
            @change="changes"
            ref="subStatus"
          />
        </div>
      </li>
      <li>
        Note:
        <textarea
          name="note"
          cols="40"
          rows="5"
          ref="subNote"
          @change="changes"
          :value="subTask.note"
        ></textarea>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "SubTask",
  props: {
    subTask: Object,
  },
  methods: {
    showDetail() {
      if (!this.haveDetail) {
        this.changeContent = false;
      }
      if (this.haveDetail && this.changeContent) {
        if (confirm("Have you saved your changes?", "yes")) {
          this.haveDetail = !this.haveDetail;
        }
      } else this.haveDetail = !this.haveDetail;
    },
    changes() {
      this.changeContent = true;
      if (this.haveDetail) {
        this.$emit("sub-content-change", [
          this.subTask.id,
          {
            title: this.$refs["subTitle"].value,
            content: this.$refs["subContent"].value,
            status: this.$refs["subStatus"].checked,
            note: this.$refs["subNote"].value,
          },
        ]);
      } else {
        this.$emit("sub-content-change", [
          this.subTask.id,
          {
            title: this.$refs["subTitle"].value,
          },
        ]);
      }
    },
  },
  components: {},
  data() {
    return {
      haveDetail: false,
      changeContent: false,
    };
  },
};
</script>
<style scoped>
.sub-task-detail input {
  width: 75%;
  margin: 5px;
  padding: 3px 7px;
  font-size: 17px;
  background: transparent;
  border: none;
  color: blue;
}
input {
  width: 140%;
  font-size: 18px;
  background: transparent;
  border: none;
}

.sub-task-detail textarea {
  background: rgb(241, 241, 167);
  width: 100%;
}
.status-box {
  display: flex;
}
.status-box label {
  flex: 1;
}
.status-box input {
  flex: 2;
  height: 20px;
}
</style>
