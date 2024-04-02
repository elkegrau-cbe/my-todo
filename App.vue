<template>
  <div id="app">
    <MyComponent />

    <header>
      <input
        v-model="newTodo.text"
        @keyup.enter="addTodo"
        placeholder="Neue Aufgabe..."
      />
      <select v-model="newTodo.priority">
        <option value="Hoch">Hoch</option>
        <option value="Mittel">Mittel</option>
        <option value="Niedrig">Niedrig</option>
      </select>
      <button @click="editingIndex === -1 ? addTodo() : saveEditedTodo()">
        {{ editingIndex === -1 ? "Hinzufügen" : "Speichern" }}
      </button>
    </header>
    <main>
      <ul>
        <li
          v-for="(todo, index) in sortedTodos"
          :key="index"
          :class="{
            completed: todo.completed,
            editing: editingIndex === index,
          }"
        >
          <input type="checkbox" v-model="todo.completed" />
          <span v-if="editingIndex !== index">{{ todo.text }}</span>
          <input
            type="text"
            v-else
            v-model="editedTodoText"
            @keyup.enter="saveEditedTodo"
            @keyup.esc="cancelEdit"
          />
          <span class="priority" :class="todo.priority">{{
            todo.priority
          }}</span>
          <button
            @click="editingIndex === index ? saveEditedTodo() : editTodo(index)"
          >
            {{ editingIndex === index ? "Speichern" : "Ändern" }}
          </button>
          <button @click="removeTodo(index)">Entfernen</button>
        </li>
      </ul>
    </main>
  </div>
</template>

<script>
import MyComponent from "./components/MyComponent.vue";

export default {
  name: "App",
  components: {
    MyComponent,
  },
  data() {
    return {
      newTodo: {
        text: "",
        priority: "Hoch",
        completed: false,
      },
      todos: [],
      editingIndex: -1,
      editedTodoText: "",
    };
  },
  computed: {
    sortedTodos() {
      return this.todos.slice().sort((a, b) => {
        const priorityOrder = { Hoch: 1, Mittel: 2, Niedrig: 3 };
        return priorityOrder[a.priority] - priorityOrder[b.priority];
      });
    },
  },
  methods: {
    addTodo() {
      if (this.newTodo.text.trim() !== "") {
        this.todos.push({ ...this.newTodo });
        this.newTodo.text = "";
        this.newTodo.priority = "Hoch";
      }
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
    },
    editTodo(index) {
      this.editingIndex = index;
      this.editedTodoText = this.todos[index].text;
    },
    saveEditedTodo() {
      if (this.editedTodoText.trim() !== "") {
        this.todos[this.editingIndex].text = this.editedTodoText.trim();
        this.editingIndex = -1;
        this.editedTodoText = "";
      }
    },
    cancelEdit() {
      this.editingIndex = -1;
      this.editedTodoText = "";
    },
  },
};
</script>

<style scoped>
#app {
  background-color: rgb(240, 238, 238);
  font-family: Arial, sans-serif;
  max-width: 650px;
  margin: 0 auto;
  padding: 20px;
}

header {
  display: flex;
  align-items: left;
  justify-content: space-between;
  margin-bottom: 20px;
}

h1 {
  font-size: 24px;
  padding-bottom: 30px;
  text-align: center;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
    Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
}

input,
select,
button {
  padding: 5px;
  font-size: 16px;
}

select {
  width: 15%;
}

main ul {
  list-style-type: none;
  padding-top: 30px;
}

li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 10px;
}

.completed span {
  text-decoration: line-through;
  color: #999;
}

.priority {
  padding: 5px 10px;
  border-radius: 5px;
  font-size: 14px;
  font-weight: bold;
}

.Hoch {
  background-color: #ff6347; /* Red */
  color: white;
}

.Mittel {
  background-color: #ffd700; /* Yellow */
}

.Niedrig {
  background-color: #52c4ee; /* Blue */
}

button {
  background-color: #52c4ee; /* Blue */
  border: none;
  color: white;
  padding: 5px 10px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 14px;
  font-weight: bold;
  margin-left: 10px;
  cursor: pointer;
  border-radius: 5px;
}

button:hover {
  background-color: #359bd1; /* Darker Green */
}
</style>
