<script setup lang="ts">
import Home from './components/Home.vue'
</script>

<template>
  <main>
    <Home />
  </main>
</template>

<style scoped>
@font-face {
  font-family: "Roboto";
  src: local("Roboto"),
   url(./fonts/Roboto-Regular.ttf) format("truetype");
}

@font-face {
  font-family: "Roboto Light";
  src: local("Roboto Light"),
   url(./fonts/Roboto-Light.ttf) format("truetype");
}

header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
