<template>
  <div class="todos">
    <h1>Deine Todos</h1>
    <h2 v-if="openTodos.length > 0">Offene Todos: {{ openTodos.length }}</h2>
    <h2 v-else>Keine Offene Todos</h2>
  </div>

  <div class="todoSection">
    <div class="addTodo">
      <input @keydown.enter="addTodo" type="text" v-model="newTodo">
      <button @click="addTodo">Add</button>
    </div>

    <hr v-if="todos.length > 0" color="515151">

    <div v-for="(todo, index) in todos" :key="todo.todoName">
      <ToDo :todoprop="todo" :todoindex="index"
      @toggledone-index="toggleDone"
      @deletetodo-index="deleteTodoItem"/>
    </div>
  </div>
</template>

<script>
import ToDo from './components/ToDo.vue';

export default {
  name: 'App',

  data() {
    return {
      newTodo: "",
      todos: [
        {
          todoName: "TODONAME", done: true
        }
      ],
    }
  },
  methods: {
    toggleDone(index) {
        this.todos[index].done = !this.todos[index].done;
        this.storeTodos();
    },
    deleteTodoItem(index) {
        this.todos.splice(index,1);
        this.storeTodos();
    },
    addTodo() {
      if(this.newTodo.trim() === "") {
        return;
      }
      this.todos.push({ todoName: this.newTodo, done: false});
      this.storeTodos();
      this.newTodo = "";
    },
    storeTodos() {
      localStorage.setItem("todos", JSON.stringify(this.openTodos));
    }
  },
  mounted() {
    let data = localStorage.getItem("todos");

    if(data !== "" && data !== null) {
      this.todos = JSON.parse(data);
    } else {
      this.todos = [];
    }
  },
  computed: {
    openTodos() {
      const openTodos = this.todos.filter((todo) => {
        return !todo.done;
      });
      return openTodos;
    },
    closedTodos() {
      const closedTodos = this.todos.filter((todo) => {
        return todo.done;
      });
      return closedTodos;
    }
  },
  components: {
    ToDo
  }
}
</script>

<style>

* {
  margin: 0;
}

.addTodo {
  display: flex;
  justify-content: space-between;
}

.addTodo input {
  width: 100vh;
  padding: 20px;
  border: 0;
  border-radius: 10px;
  background-color: rgb(0,0,0, 0.5);
  color: white;
  font-size: large;
  
}

.addTodo button {
  margin-left: 5%;
  width: 30vh;
  border-radius: 10px;
  border: 0;
  color: white;
  font-size: larger;
  background-color: rgb(0,0,0,0.5);
}

.addTodo button:hover {
  background-color: rgb(255, 255, 255, 0.1);
}



html {
  background-color: var(--dark);
  color: white;
  font-family: system-ui;
}

.todos {
  display: flex;
  background-color: rgb(0,0,0,0.75);
  padding: 15px;
  color: var(--light);
  align-items: center;
  justify-content: space-between;
}

.todoSection {
  margin: 7%;
  margin-top: 3%;
}

hr {
  margin: 20px 0;
}
</style>
