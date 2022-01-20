<template>
  <div id="app">
    <h1>TODO LIST</h1>
    <form action="" v-on:submit.prevent>
      <input type="text" v-model="newItem" />
      <input type="date" v-model="newDate" />
      <button v-on:click="addItem">送信</button>
    </form>
    <ul>
      <li v-for="(todo, key) in todos" :key="key">
        <div class="line">
          <input type="checkbox" v-model="todo.isDone" />
          <span>{{ todo.item }}</span>
          <span>期限:{{ todo.date }}</span>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data: () => ({
    newItem: "",
    newDate: "",
    todos: [],
  }),
  methods: {
    addItem: function () {
      if (this.newItem === "") return;
      let todo = {
        item: this.newItem,
        isDone: false,
        date: this.newDate,
      };
      this.todos.push(todo);
      this.newItem = "";
    },
  },
};
</script>

<style>
</style>