<template>
	<Navbar />
	<router-view />
</template>

<script>
	import Navbar from '@/components/Navbar.vue';

	export default {
		components: { Navbar },
		setup() {},
	};
</script>

<style lang="scss">
	body {
		background-color: #f2f2f2;
	}
	#app {
		font-family: Avenir, Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		max-width: 600px;
		margin: 0 auto;
		color: #555;
	}

	nav {
		padding: 30px;

		a {
			font-weight: bold;
			color: #2c3e50;

			&.router-link-exact-active {
				color: #42b983;
			}
		}
	}
</style>
