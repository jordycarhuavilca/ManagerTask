<template>
  <div>
    <input
      placeholder="Busca tu tarea"
      type="text"
      v-model="txtBuscar"
      @keyup="isEmpty"
    />
    <button @click="buscarTask">Buscar</button>
    <div v-if="resultados.length >= 1">
      <li :key="task.numTaks" v-for="task in resultados">
        {{ task.description }}
      </li>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    taskList: {
      type: [],
    },
  },
  data() {
    return {
      txtBuscar: "",
      id: 0,
      usedWords: {},
      resultados: [],
    };
  },
  methods: {
    historial() {
      let value = this.txtBuscar.trim();
      let isRepeat = false;
      const obj = {
        idWord: this.id,
        details: {
          numTaks: this.id,
          description: value,
          countRepeated: 0,
        },
      };

      const { idWord, details } = obj;
      //iterando sobre un objeto
      for (var key in this.usedWords) {
        if (this.usedWords[key].description === value) {
          isRepeat = true;
          this.usedWords[key].countRepeated++;
          break;
        }
      }
      if (!isRepeat) {
        this.usedWords[idWord] = details;
        const listObj = Object.values(this.usedWords).sort(
          (a, b) => b.countRepeated - a.countRepeated
        );
        this.id++;
        this.resultados = [...listObj];
      }
    },
    buscarTask() {
      const copiedListTask = [...this.taskList];
      const regex = new RegExp(this.txtBuscar.trim(), "i");
      this.resultados = copiedListTask.filter((r) => regex.test(r.description));
      this.historial();
    },
    isEmpty() {
      if (this.txtBuscar.trim() === "") {
        this.resultados = [];
      } else {
        const listObj = Object.values(this.usedWords).sort(
          (a, b) => b.countRepeated - a.countRepeated
        );
        this.resultados = [...listObj];
      }
    },
  },
};
</script>
