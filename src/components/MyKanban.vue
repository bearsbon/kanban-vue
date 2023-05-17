<template>
  <div>
    <div class="flex flex-col m-auto w-1/2 my-5">
      <input type="text" v-model="newTask" class="w-1/2 m-auto p-1" />
      <button
        class="hover:bg-green-400 transition duration-300 border-[1px] border-green-400 rounded-2xl m-3 py-2 px-3"
        @click="addTask()"
      >
        Добавить таску
      </button>
    </div>
    <div class="flex">
      <div class="min-h-screen overflow-x-scroll">
        <h3 class="text-gray-700 font-semibold font-sans tracking-wide text-xl">
          Todo
        </h3>
        <div class="bg-white p-3 rounded-xl">
          <draggable
            class="column"
            :list="firstColumn"
            group="cards"
            item-key="id"
            :animation="200"
            ghost-class="ghost-card"
          >
            <template #item="{ element }">
              <div :card="element" class="bg-green-400 px-5 m-2 rounded-md">
                {{ element.title }}
                <span
                  class="float-right cursor-pointer"
                  @click="deleteTask(element.id, this.firstColumn)"
                  >X</span
                >
              </div>
            </template>
          </draggable>
        </div>
      </div>

      <div class="min-h-screen overflow-x-scroll mx-5">
        <h3 class="text-gray-700 font-semibold font-sans tracking-wide text-xl">
          In process
        </h3>
        <div class="bg-white p-3 rounded-xl">
          <draggable
            class="column"
            :list="secondColumn"
            group="cards"
            item-key="id"
            :animation="200"
            ghost-class="ghost-card"
          >
            <template #item="{ element }">
              <div :card="element" class="bg-green-400 px-5 m-2 rounded-md">
                {{ element.title }}
                <span
                  class="float-right cursor-pointer"
                  @click="deleteTask(element.id, this.secondColumn)"
                  >X</span
                >
              </div>
            </template>
          </draggable>
        </div>
      </div>

      <div class="min-h-screen overflow-x-scroll mx-5">
        <h3 class="text-gray-700 font-semibold font-sans tracking-wide text-xl">
          Done
        </h3>
        <div class="bg-white p-3 rounded-xl">
          <draggable
            class="column"
            :list="thirdColumn"
            group="cards"
            item-key="id"
            :animation="200"
            ghost-class="ghost-card"
          >
            <template #item="{ element }">
              <div :card="element" class="bg-green-400 px-5 m-2 rounded-md">
                {{ element.title }}
                <span
                  class="float-right cursor-pointer"
                  @click="deleteTask(element.id, this.thirdColumn)"
                  >X</span
                >
              </div>
            </template>
          </draggable>
        </div>
      </div>

      <div class="min-h-screen overflow-x-scroll mx-5">
        <h3 class="text-gray-700 font-semibold font-sans tracking-wide text-xl">
          BackendArr
        </h3>
        <div class="bg-white p-3 rounded-xl">
          <draggable
            class="column"
            :list="backendArr"
            group="cards"
            item-key="id"
            :animation="200"
            ghost-class="ghost-card"
          >
            <template #item="{ element }">
              <div :card="element" class="bg-green-400 px-5 m-2 rounded-md">
                {{ element.title }}
                <div class="float-right">
                  <span class="cursor-pointer mr-1 text-sm">edit</span>
                  <span
                    class="float-right cursor-pointer"
                    @click="deleteTask(element.id, this.backendArr)"
                    >X</span
                  >
                </div>
              </div>
            </template>
          </draggable>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import draggable from "vuedraggable";

export default {
  name: "App",
  props: ["backendArr"],
  components: {
    draggable,
  },
  data() {
    return {
      newTask: "",
      firstColumn: [
        { id: Math.random(10), title: "aaa" },
        { id: Math.random(10), title: "qwe" },
        { id: Math.random(10), title: "www" },
      ],
      secondColumn: [
        { id: Math.random(10), title: "weqeqweq" },
        { id: Math.random(10), title: "sdmglksng" },
      ],
      thirdColumn: [
        { id: Math.random(10), title: "1312313213" },
        { id: Math.random(10), title: "1231321231231" },
      ],
      test: [],
    };
  },

  methods: {
    addTask() {
      if (this.newTask) {
        this.firstColumn.push({
          id: Math.random(10),
          title: this.newTask,
        });
        this.newTask = "";
      }
      console.log("таска добавлена");
    },
    changeInput(e) {
      this.$emit("changeInput", e.target.value);
    },
    deleteTask(id, list) {
      if (list === this.firstColumn) {
        this.firstColumn = this.firstColumn.filter((el) => el.id !== id);
      } else if (list === this.secondColumn) {
        this.secondColumn = this.secondColumn.filter((el) => el.id !== id);
      } else {
        this.thirdColumn = this.thirdColumn.filter((el) => el.id !== id);
      }
      console.log(id + " таска удалена");
    },
  },
};
</script>

<style scoped>
.ghost-card {
  opacity: 0.5;
  background: #f7fafc;
  border: 1px solid #2e38fb;
}

.column {
  width: 300px;
}
</style>
