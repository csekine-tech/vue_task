<template>
  <div id="app" class="container_wrapper">
    <div class="container">
      <h1>TODO LIST</h1>

      <form action="" v-on:submit.prevent>
        <div>
          <input type="text" v-model="newItem" />
        </div>
        <div>
          <input type="date" v-model="newDate" />
        </div>
        <div class="button_wrapper">
          <button v-on:click="addItem">送信</button>
        </div>
      </form>
      <ul>
        <li v-for="(todo, key) in todos" :key="key">
          <div class="line_wrapper">
            <div class="line">
              <input type="checkbox" v-model="todo.isDone" />
              <span>
                <span>{{ todo.item }}</span>
                <span v-if="todo.date" class="todo_date"
                  >{{ todo.date }}まで</span
                >
              </span>
            </div>
            <button v-on:click="deleteItem(key)" class="delete_button">
              X
            </button>
          </div>
        </li>
      </ul>
      <div class="button_wrapper">
        <button class="purge_button" v-on:click="purgeItem()">
          完了したタスクを削除
        </button>
      </div>
    </div>
  </div>
</template>

<script>
const today = new Date().toISOString().slice(0, 10);
export default {
  data: () => ({
    newItem: "",
    newDate: today,
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
      this.newDate = today;
    },
    deleteItem: function (key) {
      if (confirm("Are you sure?")) {
        this.todos.splice(key, 1);
      }
    },
    purgeItem: function () {
      if (confirm("Are you sure?")) {
        this.todos.forEach((todo, key) => {
          if (todo.isDone === true) {
            // console.log(todo.item);
            this.todos.splice(key, 1);
          }
        });
      }
    },
  },
};
</script>

<style>
.container {
  width: 350px;
}
.container_wrapper {
  display: flex;
  justify-content: center;
}
ul {
  list-style: none;
}
li {
  margin-left: -30px;
}
input[type="text"],
input[type="date"] {
  margin-bottom: 10px;
  width: 100%;
  padding: 5px;
  font-size: 14px;
  box-sizing: border-box;
}
button {
  border: none;
  background-color: cadetblue;
  color: white;
  padding: 8px 20px;
  cursor: pointer;
  font-size: 16px;
}
.button_wrapper {
  display: flex;
  justify-content: right;
}
.delete_button {
  padding: 8px;
  font-size: 12px;
}

.line_wrapper {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
}
input[type="checkbox"]:checked + span {
  text-decoration: line-through;
}
.todo_date {
  margin-left: 10px;
}
</style>