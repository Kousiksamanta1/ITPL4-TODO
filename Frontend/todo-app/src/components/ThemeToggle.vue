<template>
    <div class="theme-toggle" @click="toggleTheme" aria-label="Toggle theme">
      <span v-if="isLightMode">🌙</span> 
      <span v-else>☀️</span> 
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        isLightMode: true,
      };
    },
    methods: {
      toggleTheme() {
        this.isLightMode = !this.isLightMode;
        const theme = this.isLightMode ? 'light-theme' : 'dark-theme';
        localStorage.setItem('user-theme', theme);
        document.documentElement.className = theme; 
      },
    },
    mounted() {
      const savedTheme = localStorage.getItem('user-theme') || 'light-theme';
      this.isLightMode = savedTheme === 'light-theme';
      document.documentElement.className = savedTheme; 
    },
  };
  </script>
  
  <style scoped>
  .theme-toggle {
    cursor: pointer;
    font-size: 32px; 
    position: absolute; 
    top: -1px; 
    right: 20px; 
    z-index: 1; 
    transition: transform 0.3s; 
  }
  
  .theme-toggle:hover {
    transform: scale(1.1); 
  }
  </style>
  
  