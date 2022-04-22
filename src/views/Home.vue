<template>
	<div class="home">
		<FilterNav @filterChange="current = $event" :current="current" />
		<div v-if="projects.length">
			<div v-for="project in filteredProjects" :key="project.id">
				<SingleProject
					:project="project"
					@delete="handleDelete"
					@complete="handleComplete"
				/>
			</div>
		</div>
	</div>
</template>

<script>
	import SingleProject from '@/components/SingleProject.vue';
	import FilterNav from '@/components/FilterNav.vue';

	import { ref, onBeforeMount, computed } from 'vue';

	export default {
		name: 'Home',
		components: { SingleProject, FilterNav },

		setup() {
			const projects = ref([]);
			const current = ref('all');

			onBeforeMount(() => {
				fetch('http://localhost:3000/projects')
					.then((res) => res.json())
					.then((data) => (projects.value = data))
					.catch((err) => console.log(err.message));
			});

			const handleDelete = (id) => {
				projects.value = projects.value.filter((project) => {
					return project.id !== id;
				});
			};

			const handleComplete = (id) => {
				let p = projects.value.find((project) => {
					return project.id === id;
				});
				p.complete = !p.complete;
			};

			const filteredProjects = computed(() => {
				if (current.value === 'completed') {
					return projects.value.filter((project) => project.complete);
				}
				if (current.value === 'ongoing') {
					return projects.value.filter(
						(project) => !project.complete
					);
				}
				return projects.value;
			});

			return {
				projects,
				handleDelete,
				handleComplete,
				current,
				filteredProjects,
			};
		},
	};
</script>
