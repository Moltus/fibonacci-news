<script setup></script>

<template>
  <header>
    <img
      alt="Vue logo"
      class="logo"
      src="./assets/logo.svg"
      width="125"
      height="125"
    />

    <div class="wrapper">
      <h1>Fibonacci News</h1>
    </div>
  </header>

  <main>
    <p>content...</p>
  </main>
</template>

<style scoped>
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
    flex-direction: row-reverse;
    justify-content: flex-start;
  }

  .logo {
    margin: 0 2rem 0 0;
  }
}
</style>
