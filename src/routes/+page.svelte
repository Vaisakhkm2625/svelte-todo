<script>
	import { fade, crossfade } from 'svelte/transition';
	import { flip } from 'svelte/animate';

	let uid = 0;
	//let list = ['hello', 'hi'];
	let list = [];
	let [sent, receive] = crossfade({});
	let newtext = '';

	function addItem() {
		let a = {};
		a.id = uid++;
		a.done = false;
		a.name = newtext;
		list.push(a);
		list = list;
		console.log(list);
	}

	function removeItem(item) {
		list = list.filter((i) => i.id != item.id);
	}

	function handleEnter(evt) {
		if (evt.key === 'Enter') {
			//evt.srcElement.value = '';
			addItem();
			newtext = '';
		}
	}
</script>

<div class="container">
	<input on:keypress={handleEnter} bind:value={newtext} />
	<button on:click={addItem}>add</button>
	<div class="list-container">
		<div class="list">
			<div style="text-align: center;">todo</div>

			{#each list.filter((i) => i.done === false) as item (item.id)}
				<div animate:flip in:sent={{ key: item.id }} out:receive={{ key: item.id }}>
					<button
						class="todo-item list-items"
						on:click={() => {
							item.done = true;
							list = list;
						}}
					>
						{item.name}
					</button>
					<button class="x-button" on:click={removeItem(item)}> x </button>
				</div>
			{/each}
		</div>
		<div class="list">
			<div style="text-align: center;">done</div>

			{#each list.filter((i) => i.done === true) as item (item.id)}
				<div animate:flip in:sent={{ key: item.id }} out:receive={{ key: item.id }}>
					<button
						class="done-item list-items"
						on:click={() => {
							item.done = false;
							list = list;
						}}
					>
						{item.name}
					</button>
					<button class="x-button" on:click={removeItem(item)}> x </button>
				</div>
			{/each}
		</div>
	</div>
</div>

<style>
	.container {
		margin: 10px;
	}
	.list-container {
		display: flex;
		justify-content: space-evenly;
	}
	.list-items {
		width: 75%;
		padding-right: 0px;
		margin-right: 0px;
		border-top-right-radius: 0px;
		border-bottom-right-radius: 0px;
	}
	.todo-item {
		background-color: red;
		display: inline-block;
	}
	.list-item-wrapper {
		margin: 10px;
	}
	.x-button {
		margin: 0px;
		border-bottom-left-radius: 0px;
		border-top-left-radius: 0px;
	}
	.done-item {
		background-color: blue;
		display: inline-block;
	}

	.list {
		width: 45%;
	}
</style>
