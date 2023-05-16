<template>
  <div>
    <div>
      The result is:
      <p class="result">{{ inputValue }}</p>
    </div>
    <input type="text" v-model="inputValue" />
    <button @click="addItem">add item to the list</button>
  </div>
  <div class="flex gap-10">
    <div>
      <h3 class="text-lg">Дела</h3>
      <ul>
        <li
          v-for="item in list"
          v-bind:key="item.id"
          v-bind:class="{ completed: item.completed }"
        >
          <input type="checkbox" v-on:click="checked(item.id)" />
          {{ item.name }}
          <span class="pointer" v-on:click="deleteItem(item.id)">(X)</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      inputValue: "",
      list: [],
    };
  },
  methods: {
    changeInput(e) {
      this.$emit("changeInput", e.target.value);
    },
    addItem() {
      this.task = {
        id: this.list.length + 1,
        name: this.inputValue,
        completed: false,
      };
      this.list.push(this.task);
      console.log(this.task);
    },
    deleteItem(id) {
      this.list = this.list.filter((el) => el.id !== id);
    },
    checked(id) {
      this.list[id - 1].completed = !this.list[id - 1].completed;
    },
  },
};
</script>
