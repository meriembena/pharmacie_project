<template>
  <header class="todo-header">
    <h1>My Pharma</h1>
    <p class="author">by Meriam Ben Ali</p>
  </header>
</template>

<style scoped>
.todo-header {
  position: relative;
  text-align: center;
  background-color: #4caf50;
  color: white;
  padding: 40px 20px;
  border-radius: 8px;
  overflow: hidden;
}

.header-image {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  object-fit: cover;
  opacity: 0.2;
  z-index: -1;
}

.todo-header h1 {
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  font-size: 3rem;
  font-weight: bold;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
  margin: 0;
}

.author {
  font-family: "Brush Script MT", cursive;
  font-size: 1.2rem;
  font-style: italic;
  color: white;
  margin-top: 10px;
  text-shadow: 1px 1px 4px rgba(0, 0, 0, 0.6);
}
</style>
