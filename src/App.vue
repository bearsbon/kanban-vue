<template>
  <div class="wrapper">
    <!-- <div><SimpleCounter /></div>
    <div><MyTodo /></div> -->
    <div><MyKanbanVue /></div>
  </div>
</template>

<script>
import MyKanbanVue from "./components/MyKanban.vue";
// import SimpleCounter from "./components/SimpleCounter.vue";
// import MyTodo from "./components/MyTodo.vue";

export default {
  name: "App",
  components: {
    // SimpleCounter,
    // MyTodo,
    MyKanbanVue,
  },
};
</script>

<style>
.pointer {
  cursor: pointer;
}
.completed {
  text-decoration: line-through;
}
.result {
  display: inline-block;
  background: rgba(110, 103, 202, 0.511);
}
.wrapper {
  display: flex;
  justify-content: center;
  background-color: #e1e1e1;
  height: 100%;
}

input {
  border: 1px solid #333;
}
</style>
