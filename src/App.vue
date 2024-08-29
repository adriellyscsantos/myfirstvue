<template>
  <header>
    <h1 v-bind:style="{ textDecoration: state.decoration }">
      Hello {{ state.name }}, seja bem vindo(a)!
    </h1>
    <input type="text" v-model="state.name" />
  </header>
  <br />
  <br />
  <!-- <a :href="state.link" target="_blank"> Veja o meu github! </a> -->
  <section>
    <h2>Quais são as suas atividades de hoje?</h2>
    <p>(Clique duas vezes em cima da atividade para concluí-la)</p>
    <ul>
      <li
        v-for="(task, index) in state.tasks"
        @dblclick="complete(task)"
        :key="`${task}-${index}`"
        :class="{ 'line-through': task.isDone }"
        class="task-item"
      >
        {{ task.title }}
        <button class="button-remove" @click="remove(task)">&times;</button>
      </li>
    </ul>
    <h3>Digite uma nova atividade e aperte enter!</h3>
    <input
      type="text  v-focus"
      v-model="state.currentTask"
      @keyup.enter="add()"
    />
  </section>

  <article>
    <button @click="handleShowHideList()">Escolher outra frase</button>
    <h4 v-if="state.showList">
      O seu melhor é tudo o que você deve oferecer ao seu dia. Nunca entregue
      menos que isso.
    </h4>
    <h4 v-else>
      Novas oportunidades em um novo dia é tudo o que você precisa para alcançar
      seus objetivos.
    </h4>
  </article>

  <footer><span> a Vue3 website -- by Adrielly </span></footer>
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
      showList: true,
      tasks: [
        { title: "Tomar café da manhã", isDone: false },
        { title: "Trabalhar", isDone: false },
        { title: "Almoçar", isDone: false },
        { title: "Jantar", isDone: false },
        { title: "Treinar", isDone: false },
        { title: "Estudar", isDone: false },
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
body {
  background-color: #000;
  color: #fff;
  text-align: center;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

header {
  background-color: #e91e63;
  height: 100px;
}

header h1 {
  margin: 0;
  padding: 13px 0px;
}

h2 {
  color: #e91e63;
  margin-bottom: 0;
}

section {
  display: block;
  border: 1px solid #e91e63;
  padding: 0px 0px 20px 0px;
}
section p {
  font-size: 12px;
}
section ul {
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: center;
  gap: 10px;
  text-align: left;
}

section ul li {
  list-style: none;
}

article,
footer {
  margin-top: 30px;
}

button {
  background-color: #000;
  border: 1px solid #e91e63;
  color: #fff;
  padding: 10px 15px;
  border-radius: 20px;
  cursor: pointer;
}

button:hover {
  background-color: #e91e63;
  border: 1px solid #e91e63;
  transition: 0.8s;
}

.button-remove {
  background-color: #e91e63;
  color: #fff;
  border-radius: 0;
  padding: 2px 5px;
}

input {
  background-color: #f5f5f5;
  border-radius: 20px;
  border: none;
  padding: 5px 1px;
}

.line-through {
  text-decoration: line-through;
}

.task-item {
  cursor: pointer;
}
</style>
