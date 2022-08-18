<template>
<div class="panel">
  <div class="title">
    <h1>Battery</h1>
    <p>21%</p>
  </div>
  <div class="state">
    <p v-if="state === 'low'">Low battery<i class='bx bxs-plug'></i></p>
    <p v-else>Charging...<i class='bx bxs-plug'></i></p>
  </div>
  <div class="battery-container">
    <div class="battery"></div>
  </div>
</div>
</template>

<script setup>
import { ref } from 'vue';

const state = ref('low')
</script>

<style lang="scss" scoped>
.panel {
  background-color: hsl(0, 0%, 9%);
  width: min(320px, 90%);
  height: min(240px, 90%);
  border-radius: 1rem;
  padding: 2rem;
  box-sizing: border-box;
  display: grid;
  grid-template-columns: auto, 100px;
  grid-auto-rows: auto;
  .title {
    display: flex;
    flex-flow: column nowrap;
    justify-content: flex-end;
    h1 {
      font-size: 1rem;
    }
    p {
      font-size: 2rem;
    }
  }
  .state {
    display: flex;
    align-items: flex-end;
    p {
      font-size: .8rem;
    }
  }
  .battery-container {
    grid-column: 2 / 3;
    grid-row: 1 / 3;
    .battery {
      border-radius: 3rem;
      box-shadow: inset 20px 0 48px hsl(0, 0%, 16%),
        inset -4px 12px 48px hsl(0, 0%, 56%);
      width: 100%;
      height: 100%;
    }
  }
}
</style>