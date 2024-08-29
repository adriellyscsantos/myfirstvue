<template>
    <h1 v-bind:style="{ textDecoration: state.decoration }">
      Hello {{ state.name }}
    </h1>
  <input type="text" v-model="state.name" />
  <br />
  <br />
  <a :href="state.link" target="_blank"> Veja o meu github! </a>
  <h1>Minha lista de coisas</h1>
  <button @click="handleShowHideList()">Ver a lista</button>
  <ul v-if="state.showList">
    <li
      v-for="(task, index) in state.tasks"
      @dblclick="complete(task)"
      :key="`${task}-${index}`"
      :class="{ 'line-through': task.isDone }"
      class="task-item"
    >
      {{ task.title }}
      <button @click="remove(task)">&times;</button>
    </li>
  </ul>
  <p v-else>Lista escondida</p>
  <h1>Adicione sua lista de coisas!</h1>
  <input
    type="text  v-focus"
    v-model="state.currentTask"
    @keyup.enter="add()"
  />
</template>

<script>
import { reactive } from "vue";

function focus(el) {
  el.focus();
}

export default {
  directives: { focus },

  setup() {
    // Definindo os dados reativos
    const state = reactive({
      name: "Adrielly",
      decoration: "underline",
      link: "https://github.com/adriellyscsantos",
      showList: false,
      tasks: [
        { title: "HTML", isDone: false },
        { title: "CSS", isDone: false },
        { title: "JAVASCRIPT", isDone: false },
        { title: "REACT", isDone: false },
      ],
      currentTask: "",
    });

    // Método para alternar a visibilidade da lista
    const handleShowHideList = () => {
      state.showList = !state.showList;
    };

    // Método para adicionar uma nova tarefa
    const add = () => {
      if (state.currentTask.trim() === "") return; // Prevenindo tarefas vazias
      state.tasks.push({ title: state.currentTask, isDone: false });
      state.currentTask = "";
    };

    // Método para remover uma tarefa
    const remove = (task) => {
      state.tasks = state.tasks.filter((t) => t.title !== task.title);
    };

    // Método para completar uma tarefa
    const complete = (task) => {
      state.tasks = state.tasks.map((t) =>
        t.title === task.title ? { ...t, isDone: !t.isDone } : t
      );
    };

    // Retornar tudo o que será usado no template
    return {
      state,
      handleShowHideList,
      add,
      remove,
      complete,
    };
  },
};
</script>

<style>
.line-through {
  text-decoration: line-through;
}

.task-item {
  cursor: pointer;
}
</style>
