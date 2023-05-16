<template>
  <div class="hello">
    <p>{{ msg }}</p>
    <h3>{{ count }}</h3>
    <button @click="increment">+ К счетчику</button>
    <button @click="decrement">- К счетчику</button>
  </div>
</template>

<script>
export default {
  name: "SimpleCounter",
  props: {
    msg: String,
    list: Array,
  },
  data() {
    return {
      count: 0,
    };
  },
  methods: {
    increment() {
      this.count++;
    },
    decrement() {
      this.count--;
    },
  },
  mounted() {
    console.log("component mounted!!!");
  },
};
</script>

<style scoped>
.hello {
  margin: 20px;
}
</style>
