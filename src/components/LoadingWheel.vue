<template>
  <div class="loader-container">
    <h1>Loading</h1>
    <div class="loading-wheel">
      <div class="wheel-segment"></div>
      <div class="wheel-segment"></div>
      <div class="wheel-segment"></div>
      <div class="wheel-segment"></div>
    </div>
  </div>
</template>

<style scoped>
.loader-container {
  margin: auto;
}

h1 {
  text-align: center;
  color: #777;
}

.loading-wheel {
  margin: 5rem auto;
  width: 200px;
  height: 200px;
  animation: rotation 2s linear infinite;
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.wheel-segment {
  border: 4px solid #777;
  width: 90px;
  height: 90px;
}

@keyframes rotation {
  from {
    transform: rotate(0);
  }
  to {
    transform: rotate(360deg);
  }
}
</style>
