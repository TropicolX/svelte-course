<script>
	export let size = "small";
	export let shadow = false;
	export let bgColor = "inherit";
	export let textColor = "inherit";

	let isLeftHovered = false;
</script>

<button
	on:click
	style:--buttonBgColor={bgColor}
	style:--buttonTextColor={textColor}
	class:size-lg={size === "large"}
	class:size-sm={size === "small"}
	class:shadow
	{...$$restProps}
>
	{#if $$slots.leftContent}
		<div
			class="left-content"
			on:mouseenter={() => (isLeftHovered = true)}
			on:mouseleave={() => (isLeftHovered = false)}
		>
			<slot name="leftContent" />
		</div>
	{/if}
	<slot {isLeftHovered}>Fallback</slot>
</button>

<style lang="scss">
	button {
		display: flex;
		align-items: center;
		border: none;
		background-color: var(--buttonBgColor);
		color: var(--buttonTextColor);
		padding: 15px 20px;
		font-weight: bold;
		border-radius: 5px;
		cursor: pointer;

		&:hover {
			background-color: darken(#ff3e00, 20%);
		}

		&:active {
			background-color: darken(#ff3e00, 5%);
		}

		&:disabled {
			background-color: #ccc;
			color: #666;
			cursor: not-allowed;
		}

		&.size-sm {
			padding: 15px 20px;
		}

		&.size-lg {
			padding: 20px 25px;
			font-size: 20px;
		}

		&.shadow {
			box-shadow: 0 0 10px rgba(1, 1, 1, 0.3);
		}

		.left-content {
			margin-right: 10px;
		}
	}
</style>
