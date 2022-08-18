<template>
  <h1 class="title">{{title}}</h1>
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
  text-align: center;
}
</style>
