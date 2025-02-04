<template>
    <div class="todo-container" :class="{ 'dark-theme': isDarkMode }">
      <h1>To-Do List</h1>
      <div class="input-container">
        <input
          v-model="newTodo"
          @keyup.enter="addTodo"
          placeholder="Add a new task"
          class="task-input"
        />
        <button @click="addTodo" class="add-button">
          <i class="fas fa-check"></i> 
        </button>
      </div>
      <ul>
        <li v-for="(todo, index) in todos" :key="todo.id">
          <span v-if="!todo.isEditing">{{ todo.task }}</span>
          <input v-if="todo.isEditing" v-model="todo.task" />
          <button v-if="!todo.isEditing" @click="editTodo(index)" class="edit-button">
            <i class="fas fa-edit"></i> 
          </button>
          <button v-if="todo.isEditing" @click="saveTodo(index)" class="save-button">
            <i class="fas fa-save"></i> 
          </button>
          <button @click="deleteTodo(index)" class="delete-button">
            <i class="fas fa-times"></i> 
          </button>
        </li>
      </ul>
      <p v-if="errorMessage" class="error">{{ errorMessage }}</p>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        newTodo: '',
        todos: [],
        errorMessage: '',
        isDarkMode: false, 
      };
    },
    methods: {
      async fetchTodos() {
        try {
          const response = await axios.get('http://127.0.0.1:5000/api/todos');
          this.todos = response.data.map(todo => ({ ...todo, isEditing: false })); 
        } catch (error) {
          this.errorMessage = 'Failed to fetch todos.';
          console.error('Error fetching todos:', error);
        }
      },
      async addTodo() {
        if (this.newTodo.trim() === '') return;
  
        try {
          const todo = { task: this.newTodo };
          const response = await axios.post('http://127.0.0.1:5000/api/todos', todo);
          this.todos.push({ ...response.data, isEditing: false }); 
          this.newTodo = '';
        } catch (error) {
          this.errorMessage = 'Failed to add todo.';
          console.error('Error adding todo:', error);
        }
      },
      async deleteTodo(index) {
        try {
          await axios.delete(`http://127.0.0.1:5000/api/todos/${this.todos[index].id}`);
          this.todos.splice(index, 1); 
        } catch (error) {
          this.errorMessage = 'Failed to delete todo.';
          console.error('Error deleting todo:', error);
        }
      },
      editTodo(index) {
        this.todos[index].isEditing = true; 
      },
      async saveTodo(index) {
        try {
          await axios.put(`http://127.0.0.1:5000/api/todos/${this.todos[index].id}`, this.todos[index]);
          this.todos[index].isEditing = false; 
        } catch (error) {
          this.errorMessage = 'Failed to save todo.';
          console.error('Error saving todo:', error);
        }
      },
      toggleTheme() {
        this.isDarkMode = !this.isDarkMode; 
      }
    },
    mounted() {
      this.fetchTodos();
    }
  };
  </script>
  
  <style scoped>
  .todo-container {
    max-width: 600px;
    margin: auto;
    padding: 20px;
    border: 1px solid #ccc;
    border-radius: 8px;
    background-color: #f9f9f9; 
    transition: background-color 0.5s, color 0.5s; 
  }
  
  .dark-theme {
    background-color: #333; 
    color: black; 
  }
  
  h1 {
    color: rgb(0, 0, 0); 
    font-display: block;
    font-weight: bold;
  }
  
  .input-container {
    display: flex;
    align-items: center;
    margin-bottom: 20px;
  }
  
  .task-input {
    flex: 1;
    padding: 10px;
    border: 2px solid #007bff; 
    border-radius: 5px;
    font-size: 16px;
    transition: border-color 0.3s;
  }
  
  .task-input:focus {
    border-color: #0056b3; 
    outline: none; 
  }
  
  .add-button {
    background-color: #007bff; 
    color: white;
    border: none;
    border-radius: 5px;
    padding: 10px 15px;
    margin-left: 10px;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  
  .add-button:hover {
    background-color: #0056b3; 
  }
  
  ul {
    list-style-type: none;
    padding: 0;
  }
  
  li {
    display: flex;
    font-style: italic;
    color: #000000;
    justify-content: space-between;
    align-items: center;
    padding: 10px 0;
    border-bottom: 1px solid #000000;
  }
  
  .edit-button, .delete-button, .save-button {
    font-size: 24px; 
    background: none;
    border: none;
    cursor: pointer;
  }
  
  .save-button {
    font-size: 24px; 
  }
  
  .error {
    color: red;
    margin-top: 10px;
  }
  </style>
  
  