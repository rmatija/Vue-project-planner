<template>
	<div>
		<form @submit.prevent="handleEdit">
			<label>Title</label>
			<input type="text" v-model="title" required />
			<label>Details</label>
			<textarea v-model="details" required></textarea>
			<button>Update Project</button>
		</form>
	</div>
</template>

<script>
	import { onBeforeMount, ref } from 'vue';
	import { useRouter } from 'vue-router';

	export default {
		props: ['id'],

		setup(props) {
			const title = ref('');
			const details = ref('');
			const uri = ref('http://localhost:3000/projects/' + props.id);
			const router = useRouter();

			onBeforeMount(() => {
				fetch(uri.value)
					.then((res) => res.json())
					.then((data) => {
						title.value = data.title;
						details.value = data.details;
					})
					.catch((err) => console.log(err.message));
			});

			const handleEdit = () => {
				fetch(uri.value, {
					method: 'PATCH',
					headers: { 'Content-Type': 'application/json' },
					body: JSON.stringify({
						title: title.value,
						details: details.value,
					}),
				})
					.then(() => {
						router.push('/');
					})
					.catch((err) => console.log(err));
			};

			return {
				title,
				details,
				uri,
				handleEdit,
			};
		},
	};
</script>

<style scoped lang="scss">
	form {
		background: white;
		padding: 20px;
		border-radius: 10px;

		label {
			display: block;
			color: #bbb;
			text-transform: uppercase;
			font-size: 14px;
			font-weight: bold;
			letter-spacing: 1px;
			margin: 20px 0 10px 0;
		}

		input {
			padding: 10px;
			border: 0;
			border-bottom: 1px solid #ddd;
			width: 100%;
			box-sizing: border-box;
		}

		textarea {
			border: 1px solid #ddd;
			padding: 10px;
			width: 100%;
			box-sizing: border-box;
			height: 100px;
		}

		button {
			display: block;
			margin: 20px auto 0;
			background: #00ce89;
			color: white;
			padding: 10px;
			border: 0;
			border-radius: 6px;
			font-size: 16px;
		}
	}
</style>
