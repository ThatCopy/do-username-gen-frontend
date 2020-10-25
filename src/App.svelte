<script>
import { onMount } from "svelte";
	let promise = getDoUsername();
	let bodyClass = document.body.classList; // Attributes in Class
	let isDarkMode;

	onMount(() => {
	    let theme_config = JSON.parse(localStorage.getItem('DarkMode')); // return a Boolean from Local Storage
		if (theme_config) {
			setDarkMode()
		}
	})

	async function getDoUsername() {
		const res = await fetch(`https://thatcopy.pw/do/`);
		const text = await res.text();

		if (res.ok) {
			return text;
		} else {
			throw new Error(text);
		}
	}

	function handleClick() {
		promise = getDoUsername();
	}

	function setDarkMode() {
		bodyClass.toggle('dark');
		isDarkMode = bodyClass.contains("dark");
		localStorage.setItem('DarkMode', isDarkMode);
	}
</script>

<main>
	<div id="wrap">
	<button on:click={handleClick}>
		Get a random DigitalOcean username
	</button>

	{#await promise}
		<h1>...loading</h1>
	{:then username}
		<h1>{username}</h1>
	{:catch error}
		<h1 id="error" style="color: red">{error.message}</h1>
	{/await}
	<button id="darkmode" on:click={setDarkMode}>
		DarkMode: {isDarkMode ? "ON" : "OFF"}
	</button>
</div>
</main>

<style>
	#wrap{
		display: flex;
		width: 30%;
  		margin-left: auto;
  		margin-right: auto;
		justify-content: center;
		flex-direction: column;
	}
	h1{
		text-align: center;
	}
	
</style>
