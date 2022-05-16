<template>
  <transition name="fade">
    <router-view></router-view>
  </transition>
</template>

<script>
  export default {
    beforeDestroy () {
      window.sessionStorage.clear()
    }
  }
</script>
