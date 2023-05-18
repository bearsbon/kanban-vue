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
            id="todoColumn"
            class="column"
            :list="todoColumn"
            group="cards"
            item-key="id"
            :animation="200"
            ghost-class="ghost-card"
            @change="patch($event, 4)"
          >
            <template #item="{ element }">
              <div
                :card="element"
                class="bg-green-400 px-5 py-2 m-2 rounded-md"
              >
                {{ element.title }}
                <div class="float-right">
                  <span class="cursor-pointer mr-1 text-sm">edit</span>
                  <span
                    class="float-right cursor-pointer"
                    @click="deleteB(element.id, this.todoColumn)"
                    >X</span
                  >
                </div>
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
            id="inProcessColumn"
            class="column"
            :list="inProcessColumn"
            group="cards"
            item-key="id"
            :animation="200"
            ghost-class="ghost-card"
            @change="patch($event, 5)"
          >
            <template #item="{ element }">
              <div
                :card="element"
                class="bg-green-400 px-5 py-2 m-2 rounded-md"
              >
                {{ element.title }}
                <div class="float-right">
                  <span class="cursor-pointer mr-1 text-sm">edit</span>
                  <span
                    class="float-right cursor-pointer"
                    @click="deleteB(element.id, this.inProcessColumn)"
                    >X</span
                  >
                </div>
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
            id="doneColumn"
            class="column"
            :list="doneColumn"
            group="cards"
            item-key="id"
            :animation="200"
            ghost-class="ghost-card"
            @change="patch($event, 6, this.doneColumn)"
          >
            <template #item="{ element }">
              <div
                :card="element"
                class="bg-green-400 px-5 py-2 m-2 rounded-md"
              >
                {{ element.title }}
                <div class="float-right">
                  <span class="cursor-pointer mr-1 text-sm">edit</span>
                  <span
                    class="float-right cursor-pointer"
                    @click="deleteB(element.id, this.doneColumn)"
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
import axios from "axios";
import { toRaw } from "vue";

export default {
  name: "App",
  components: {
    draggable,
  },
  data() {
    return {
      newTask: "",
      todoColumn: [],
      inProcessColumn: [],
      doneColumn: [],
    };
  },

  methods: {
    async addTask() {
      if (this.newTask) {
        let task = {
          id: 0,
          title: this.newTask,
          column_id: 4,
          index: this.todoColumn.length,
        };
        const res = await axios.post(`http://127.0.0.1:8000/tasks/`, task);
        task.id = res.data.id;
        this.todoColumn.push(task);
        this.newTask = "";
      }
      console.log("таска добавлена");
    },
    changeInput(e) {
      this.$emit("changeInput", e.target.value);
    },
    deleteB(id, arr) {
      axios.delete(`http://127.0.0.1:8000/tasks/${id}`);
      if (arr === this.todoColumn)
        this.todoColumn = this.todoColumn.filter((el) => el.id !== id);
      if (arr === this.inProcessColumn)
        this.inProcessColumn = this.inProcessColumn.filter(
          (el) => el.id !== id
        );
      if (arr === this.doneColumn)
        this.doneColumn = this.doneColumn.filter((el) => el.id !== id);
    },
    patch(event, b) {
      let rawData;
      let newIndex;
      const raw = toRaw(event);
      if (raw.added) {
        rawData = toRaw(raw.added.element);
        newIndex = raw.added.newIndex;
      }
      if (raw.moved) {
        rawData = toRaw(raw.moved.element);
        newIndex = raw.moved.newIndex;
      }
      if (rawData)
        axios.patch(`http://127.0.0.1:8000/tasks/${rawData.id}`, {
          ...rawData,
          column_id: b,
          index: newIndex,
        });
    },
    log(event) {
      console.log(event);
    },
  },
  async created() {
    axios.get("http://127.0.0.1:8000/tasks/").then((res) => {
      res.data.map((el) => {
        if (el.column_id === 4) this.todoColumn.splice(el.index, 0, el);
        if (el.column_id === 5) this.inProcessColumn.splice(el.index, 0, el);
        if (el.column_id === 6) this.doneColumn.splice(el.index, 0, el);
      });
    });
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
