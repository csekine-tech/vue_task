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
        <li v-for="(todo, key) in reverseTodos" :key="key">
          <div class="line_wrapper">
            <div class="line">
              <input type="checkbox" v-model="todo.isDone" />
              <span>
                <span>{{ todo.item }}</span>
                <span v-if="todo.remainDay < -365" class="todo_date red">
                  {{ absNumber(todo.remainYear) }}年前
                </span>
                <span v-else-if="todo.remainDay < -30" class="todo_date red" >
                  {{ absNumber(todo.remainMonth) }}カ月前
                </span>
                <span v-else-if="todo.remainDay < -1" class="todo_date red">
                  {{ absNumber(todo.remainDay) }}日前
                </span>
                <span v-else-if="todo.remainDay === -1" class="todo_date red">
                  昨日
                </span>
                <span v-else-if="todo.remainDay === 0" class="todo_date red">
                  今日
                </span>
                <span v-else-if="todo.remainDay === 1" class="todo_date">
                  明日
                </span>
                <span v-else-if="todo.remainDay > 365" class="todo_date">
                  {{ todo.remainYear }}年後
                </span>
                <span v-else-if="todo.remainDay > 30" class="todo_date">
                  {{ todo.remainMonth }}カ月後
                </span>
                <span v-else-if="todo.remainDay > 1" class="todo_date">
                  {{ todo.remainDay }}日後
                </span>
              </span>
            </div>
            <button v-on:click="deleteItem(key)" class="delete_button">
              X
            </button>
          </div>
        </li>
      </ul>
      <div class="button_wrapper">
        <button
          class="purge_button"
          v-on:click="purgeItem()"
          v-if="todos.length"
        >
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
    reverseTodos: [],
    // remainDay: [],
  }),
  methods: {
    addItem: function () {
      if (this.newItem === "") return;
      let todo = {
        item: this.newItem,
        isDone: false,
        date: this.newDate,
        remainDay: this.remainDay,
        remainMonth: this.remainDay / 30,
        remainYear: this.remainDay / 365,
      };
      var todayTime = new Date(today);
      var newDateTime = new Date(this.newDate);
      todo.remainDay = (newDateTime - todayTime) / 86400000;
      todo.remainMonth = Math.ceil((newDateTime - todayTime) / 86400000 / 30);
      todo.remainYear = Math.ceil((newDateTime - todayTime) / 86400000 / 365);
      this.todos.push(todo);
      this.reverseTodos = this.todos.slice().reverse();
      this.newItem = "";
      console.log(this.remainDay);
      this.newDate = today;
    },
    deleteItem: function (key) {
      if (confirm("Are you sure?")) {
        this.todos.splice(length - key - 1, 1);
        this.reverseTodos = this.todos.slice().reverse();
      }
    },
    purgeItem: function () {
      if (!this.todos.length) return;
      if (confirm("Are you sure?")) {
        this.todos.forEach((todo, key) => {
          if (todo.isDone === true) {
            this.todos.splice(key, 1);
            this.reverseTodos = this.todos.slice().reverse();
            console.log(key);
          }
        });
      }
    },
    absNumber: function (num) {
      return Math.abs(num);
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
.red{
  color: red;
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