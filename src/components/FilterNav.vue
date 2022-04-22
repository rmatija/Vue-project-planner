<template>
	<nav class="filter-nav">
		<button
			@click="updateFilter('all')"
			:class="{ active: current === 'all' }"
		>
			Viev all
		</button>
		<button
			@click="updateFilter('completed')"
			:class="{ active: current === 'completed' }"
		>
			Completed
		</button>
		<button
			@click="updateFilter('ongoing')"
			:class="{ active: current === 'ongoing' }"
		>
			Ongoing
		</button>
	</nav>
</template>

<script>
	export default {
		props: ['current'],

		setup(props, context) {
			const updateFilter = (filteringBy) => {
				context.emit('filterChange', filteringBy);
			};

			return {
				updateFilter,
			};
		},
	};
</script>

<style lang="scss" scoped>
	.filter-nav {
		button {
			background: none;
			border: none;
			color: #bbb;
			outline: none;
			font-size: 12px;
			text-transform: uppercase;
			margin-right: 10px;
			letter-spacing: 1px;
			font-weight: bold;
			cursor: pointer;

			&.active {
				color: #555;
			}
		}
	}
</style>
