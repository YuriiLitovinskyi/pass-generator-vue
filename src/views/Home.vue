<template>
  <div class="home">
    <img alt="Vue logo" class="logo" src="../assets/logo.png">
    <PassGenerator msg="Password Generator"/>   
  </div>
</template>

<script>
// @ is an alias to /src
import PassGenerator from '@/components/PassGenerator.vue'

export default {
  name: 'Home',
  components: {
    PassGenerator 
  }
}
</script>

<style scoped>
  .logo {
    max-height: 60px;
  }
</style>
