<script>
	import Button from "./Button.svelte";
	import { v4 as uuid } from "uuid";

	export let todos = [];

	let inputText = "";
	const handleAddTodo = () => {
		todos = [
			...todos,
			{
				id: uuid(),
				title: inputText,
				completed: false,
			},
		];
	};
</script>

<div class="todo-list-wrapper">
	<ul>
		{#each todos as { id, title }, index (id)}
			{@const number = index + 1}
			<li>{number} - {title}</li>
		{/each}
	</ul>
	<form class="add-todo-form" on:submit|preventDefault={handleAddTodo}>
		<input bind:value={inputText} />
		<Button type="submit" disabled={!inputText}>Add</Button>
	</form>
</div>

<style>
</style>
