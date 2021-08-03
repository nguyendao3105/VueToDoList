<template>
  <form @submit="onSubmit" class="add-form">
    <div class="form-control">
      <label>Title</label>
      <input type="text" v-model="title" name="title" placeholder="Title" />
    </div>
    <div class="form-control">
      <label>Content</label>
      <input
        type="text"
        v-model="content"
        name="content"
        placeholder="Content"
      />
    </div>
    <div class="form-control">
      <label>Note</label>
      <input type="text" v-model="note" name="note" placeholder="Note" />
    </div>

    <input type="submit" value="Add" class="btn btn-block" />
  </form>
</template>

<script>
export default {
  name: "AddSubTask",
  data() {
    return {
      title: "",
      content: "",
      note: "",
    };
  },
  methods: {
    onSubmit(e) {
      e.preventDefault();

      if (!this.title || !this.content) {
        alert("Please enter all title and content");
        return;
      }

      const newSubTask = {
        id: Math.floor(Math.random() * 10000),
        title: this.title,
        content: this.content,
        status: false,
        note: this.note,
      };

      this.$emit("add-sub-task", newSubTask);
    },
  },
};
</script>
