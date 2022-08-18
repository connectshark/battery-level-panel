<template>
  <router-view></router-view>
</template>

<script>
export default {
  setup () {
    return {
      title: 'Hello'
    }
  }
}
</script>


<style lang="scss">
#app {
  font-family: 'Roboto Mono', 'Noto Sans TC', sans-serif;
  background-color: hsl(0, 0%, 15%);
  min-height: 100vh;
  display: grid;
  place-items: center;
  color: #fff;
}
</style>
