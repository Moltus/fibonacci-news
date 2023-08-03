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
  width: 100px;
  height: 100px;
  animation: rotation 2s linear infinite;
  display: flex;
  flex-wrap: wrap;
  gap: 5px;
}

.wheel-segment {
  border: 2px solid #666;
  width: 47.5px;
  height: 47.5px;
}

.wheel-segment:nth-child(1) {
  border-top: none;
  border-right: none;
  border-radius: 100% 0 0 0;
  background-color: #202020;
}

.wheel-segment:nth-child(2) {
  border-right: none;
  border-bottom: none;
  border-radius: 0 100% 0 0;
  background-color: #282828;
}

.wheel-segment:nth-child(3) {
  border-left: none;
  border-top: none;
  border-radius: 0 0 0 100%;
  background-color: #303030;
}

.wheel-segment:nth-child(4) {
  border-bottom: none;
  border-left: none;
  border-radius: 0 0 100% 0;
  background-color: #383838;
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
