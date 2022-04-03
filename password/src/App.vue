<template>
<PasswordForm />
</template>

<script>
import PasswordForm from './components/PasswordForm.vue'

export default {
  name: 'App',
  components: {
    PasswordForm
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

body {
  margin: 0;
  background: #eee;
}
</style>
