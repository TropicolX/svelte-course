<svelte:options immutable />

<script>
	import Button from "./Button.svelte";
	import { createEventDispatcher, onMount, tick } from "svelte";

	onMount(() => {
		autoScroll();

		return () => {
			console.log("Unmounted");
		};
	});

	const autoScroll = () => {
		listDiv.scrollTo(0, listDiv.scrollHeight);
	};

	export let todos = [];
	export const clearInput = () => {
		inputText = "";
	};
	export const focusInput = () => {
		input.focus();
	};

	let input, listDiv;

	let inputText = "";

	const dispatch = createEventDispatcher();

	const handleAddTodo = async () => {
		const isNotCancelled = dispatch(
			"addtodo",
			{ title: inputText },
			{ cancelable: true }
		);

		await tick();

		autoScroll();

		if (isNotCancelled) {
			inputText = "";
		}
	};

	const handleRemoveTodo = (id) => {
		dispatch("removetodo", { id });
	};

	const handleToggleTodo = (id) => {
		dispatch("toggletodo", { id });
	};
</script>

<div class="todo-list-wrapper">
	<div class="todo-list" bind:this={listDiv}>
		<ul>
			{#each todos as { id, title, completed } (id)}
				<li>
					<label>
						<input
							on:input={(event) =>
								(event.currentTarget.checked = completed)}
							on:change={() => handleToggleTodo(id)}
							type="checkbox"
							checked={completed}
						/>
						{title}
					</label>
					<button on:click={() => handleRemoveTodo(id)}>Remove</button
					>
				</li>
			{/each}
		</ul>
	</div>
	<form class="add-todo-form" on:submit|preventDefault={handleAddTodo}>
		<input bind:this={input} bind:value={inputText} />
		<Button type="submit" disabled={!inputText}>Add</Button>
	</form>
</div>

<style>
	.todo-list {
		max-width: 200px;
		max-height: 150px;
		overflow: auto;
	}
</style>
