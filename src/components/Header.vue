<template>
  <header>
    <h1>{{ title }}</h1>
    <Button
      @btn-click="$emit('show-add-form')"
      :text="haveAddForm ? 'Close' : 'Add Task'"
      :color="haveAddForm ? 'red' : 'green'"
    />
  </header>
</template>

<script>
import Button from "./Button";
export default {
  name: "Header",
  props: {
    title: String,
    haveAddForm: Boolean,
  },
  components: {
    Button,
  },
};
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
</style>
