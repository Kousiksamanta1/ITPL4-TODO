<template>
  <div id="app" :class="userTheme">
    <ThemeToggle @theme-changed="updateTheme" />
    <h1>Welcome to My Todo App</h1>
    <TodoList />
  </div>
</template>

<script>
import TodoList from './components/TodoList.vue';
import ThemeToggle from './components/ThemeToggle.vue';
import '@fortawesome/fontawesome-free/css/all.css';

export default {
  components: {
    TodoList,
    ThemeToggle,
  },
  data() {
    return {
      userTheme: localStorage.getItem("user-theme") || 'light-theme',
    };
  },
  methods: {
    updateTheme(newTheme) {
      this.userTheme = newTheme;
      document.documentElement.className = newTheme; 
    },
  },
  mounted() {
    document.documentElement.className = this.userTheme;
  },
};
</script>

<style>
html, body {
  height: 100%; 
  margin: 0; 
}

#app {
  position: relative; 
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  height: 100%; 
}


#app::before {
  content: '';
  position: fixed; 
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-image: url('@/assets/1.jpg'); 
  background-size: cover; 
  background-position: center; 
  background-repeat: no-repeat; 
  opacity: 0.5; 
  z-index: -1; 
}


:root {
  --background-color: #ffffff; 
  --text-color: #000000; 
}

.dark-theme {
  --background-color: #000000; 
  --text-color: #ffffff; 
}

body {
  background-color: var(--background-color);
  color: var(--text-color);
  transition: background-color 0.5s, color 0.5s;
}
</style>

