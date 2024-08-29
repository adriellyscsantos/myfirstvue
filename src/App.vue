<template>
  <div>
    <h1 v-bind:style="{ textDecoration: decoration }">Hello {{ name }}</h1>
  </div>
  <input type="text" v-model="name" />
  <br />
  <br />
  <a :href="link" target="_blank"> Veja o meu github! </a>
  <h1>Minha lista de coisas</h1>
  <button @click="handleShowHideList()"> Ver a lista</button>
  <ul v-if="showList">
    <li 
    v-for="(task, index) in tasks" 
    @dblclick="complete(task)"
    :key="`${task}-${index}`"
    :class="{'line-through': !task.isDone}"
    class='task-item'
    >
      {{ task.title }}
      <button @click="remove(task)"> &times; </button>
    </li>
  </ul>
  <p v-else> Lista escondida </p>

  <input type="text" v-focus />
</template>

<script>
const focus = (el) =>{
  el.focus();
}
export default {
  directives: { focus },
  data: () => ({
    name: "Adrielly",
    decoration: "underline",
    link: "https://github.com/adriellyscsantos",
    showList: false,
    tasks: [
      { title: "HTML", isDone: "false" },
      { title: "CSS", isDone: "false" },
      { title: "JAVASCRIPT", isDone: "false" },
      { title: "REACT", isDone: "false" },
    ],
  }),
  methods: {
    handleShowHideList() {
    this.showList = !this.showList
    },
    remove(task) {
      this.tasks = this.tasks.filter(t => t.title !== task.title)
      // console.log('task', task)
    },
    complete(task){
      this.tasks = this.tasks.map(t=>{
        if(t.title === task.title){
          return {...t, isDone: !t.isDone}
          }
          return {...t}
      })
    }
  }
};
</script>

<style>
.line-through{
  text-decoration: line-through;
}

.task-item{
  cursor: pointer;
}
</style>
