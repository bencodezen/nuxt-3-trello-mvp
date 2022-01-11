<script>
export default {}
</script>

<template>
  <div>
    <header class="main-header">
      <p class="logo">Trello MVP Clone with Nuxt 3</p>
      <nav class="main-nav">
        <nuxt-link to="/">Home</nuxt-link>
      </nav>
    </header>

    <slot />
  </div>
</template>

<style>
html,
body {
  margin: 0;
  padding: 0;
  font-family: 'Roboto', sans-serif;
}

.main-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #41b883;
  padding: 15px;
}

.main-nav a {
  color: #fff;
  text-decoration: none;
}

.logo {
  color: #fff;
  font-weight: bold;
  margin: 0;
  padding: 0;
}
</style>
