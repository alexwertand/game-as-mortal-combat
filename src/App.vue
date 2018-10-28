<template>
	<main id="app">
		<simple-game-like-mortal-combat />
	</main>
</template>

<script>
import SimpleGameLikeMortalCombat from './components/SimpleGameLikeMortalCombat.vue'

export default {
  name: 'app',
  components: {
    SimpleGameLikeMortalCombat
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
