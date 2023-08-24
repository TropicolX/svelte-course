<script>
	import TodoList from "./lib/TodoList.svelte";
	import { v4 as uuid } from "uuid";

	let todoList;

	let todos = [
		{
			id: uuid(),
			title: "Todo 1",
			completed: true,
		},
		{
			id: uuid(),
			title: "Todo 2",
			completed: false,
		},
		{
			id: uuid(),
			title: "Todo 3",
			completed: true,
		},
	];

	let todoMounted = true;

	const toggleTodoMounted = () => {
		todoMounted = !todoMounted;
	};

	const handleAddTodo = async (event) => {
		event.preventDefault();
		todos = [
			...todos,
			{
				id: uuid(),
				title: event.detail.title,
				completed: false,
			},
		];
		todoList.clearInput();
	};

	const handleRemoveTodo = (event) => {
		todos = todos.filter((todo) => todo.id !== event.detail.id);
	};

	const handleToggleTodo = (event) => {
		todos = todos.map((todo) =>
			todo.id === event.detail.id
				? { ...todo, completed: !todo.completed }
				: todo
		);
	};
</script>

{#if todoMounted}
	<TodoList
		{todos}
		bind:this={todoList}
		on:addtodo={handleAddTodo}
		on:removetodo={handleRemoveTodo}
		on:toggletodo={handleToggleTodo}
	/>
{/if}
<button on:click={() => todoList.focusInput()}>Focus input</button>
<button on:click={toggleTodoMounted}>{todoMounted ? "Unmount" : "Mount"}</button
>

<style>
</style>
