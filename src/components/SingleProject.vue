<template>
	<div class="project" :class="{ completed: project.complete }">
		<div class="actions">
			<h3
				@click="showHide(), changeColor()"
				:class="clicked ? 'black' : 'red'"
			>
				{{ project.title }}
			</h3>
			<div class="icons">
				<span @click="toggleComplete" class="material-icons tick"
					>done</span
				>
				<router-link
					:to="{ name: 'EditProject', params: { id: project.id } }"
				>
					<span class="material-icons">edit</span>
				</router-link>
				<span @click="deleteProject" class="material-icons"
					>delete</span
				>
			</div>
		</div>
		<div v-if="showDetails" class="details">
			<p>{{ project.details }}</p>
		</div>
	</div>
</template>

<script>
	import { ref } from 'vue';

	export default {
		props: ['project'],

		setup(props, { emit }) {
			const showDetails = ref(false);
			const clicked = ref(true);
			const uri = ref(
				'http://localhost:3000/projects/' + props.project.id
			);

			const showHide = () => {
				showDetails.value = !showDetails.value;
			};

			const changeColor = () => {
				clicked.value = !clicked.value;
			};

			const deleteProject = () => {
				fetch(uri.value, { method: 'DELETE' })
					.then(() => emit('delete', props.project.id))
					.catch((err) => console.log(err));
			};

			const toggleComplete = () => {
				fetch(uri.value, {
					method: 'PATCH',
					headers: { 'Content-Type': 'application/json' },
					body: JSON.stringify({ complete: !props.project.complete }),
				})
					.then(() => emit('complete', props.project.id))
					.catch((err) => console.log(err));
			};

			return {
				showDetails,
				clicked,
				showHide,
				changeColor,
				deleteProject,
				uri,
				emit,
				toggleComplete,
			};
		},
	};
</script>

<style scoped lang="scss">
	.project {
		margin: 20px auto;
		background-color: white;
		padding: 10px 20px;
		border-radius: 4px;
		box-shadow: 1px 2px 3 px rgba(0, 0, 0, 0.05);
		border-left: 4px solid #e90074;

		h3 {
			cursor: pointer;
		}

		.black {
			color: black;
		}
		.red {
			color: red;
		}

		.actions {
			display: flex;
			justify-content: space-between;
			align-items: center;

			.material-icons {
				font-size: 24px;
				margin-left: 10px;
				color: #bbb;
				cursor: pointer;

				&:hover {
					color: #777;
				}
			}
		}

		&.completed {
			border-left: 4px solid #00ce89;

			.tick {
				color: #00ce89;
			}
		}
	}
</style>
