<template>
  <div>
    <div>
      <CustomSearchInput
      :taskList="taskList"
      />
    </div>
    <div>
      <input
        type="text"
        v-model="description"
        required
        @keyup.enter="addTask"
      />
      <button @click="addTask">Agregar</button>
    </div>
    <div>
      <CustomList
        v-for="(task, index) in taskList"
        :key="index"
        :description="task.description"
        @remove="taskList.splice(index, 1)"
        @update="updateTask(index)"
      />
    </div>
  </div>
</template>

<script>
import CustomList from "./CustomList.vue";
import CustomSearchInput from "./CustomSearchInput.vue"
export default {
  components: {
    CustomList,
    CustomSearchInput
  },
  data() {
    return {
      taskList: [],
      numTaks: 0,
      description: ""
    };
  },
  methods: {
    serializedJson() {
      let json = JSON.stringify(this.taskList);
      localStorage.setItem("taskList", json);
    },
    addTask() {
      this.taskList.push({
        numTaks: this.numTaks,
        description: this.description,
      });
      this.serializedJson();
      this.numTaks++;
    },
    updateTask(index) {
      this.taskList[index].description = this.description;
      this.serializedJson();
    },
  },
};
</script>
