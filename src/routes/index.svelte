<script>
	import {onMount} from 'svelte';
	onMount(() => {
		document.getElementById('search').focus();
	})
	let gifs = [];
	let keyword='';
	let loading = false;
	$: API_KEY = 'O0PIjSQKNXlhQl4ro8ngi3paQujecP6C';
	function getGifs(event){
		loading = true;
		gifs = [];
		if(event) event.preventDefault();
		fetch(`https://api.giphy.com/v1/gifs/search?api_key=${API_KEY}&q=${keyword}&limit=25&offset=0&rating=G&lang=en`)
		.then(res => res.json())
		.then(json => gifs = json.data).catch(err => getGifs());
		loading = false;
	}

</script>
<style>
form {
	display: flex;
	margin: 10px 0px;
}
input {
	padding: 8px 10px;
	border-radius: 5px;
	border: 1px solid gray;
	box-sizing: border-box;
	width: 100%;
	outline: 0;
}
input[type="submit"]{
	width: 70px;
	color: white;
	background: black;
	border: 0;
	border-radius: 0;
}
.images {
	column-count: 4;
}
img {
	width: 100%;
	height: auto;
	background: #c9c9c9;
	min-width: 200px;
	min-height: 200px;
}
@media screen and (max-width: 1024px){
	.images {
		column-count: 3;
	}
}
@media screen and (max-width: 700px){
	.images {
		column-count: 2;
	}
}
@media screen and (max-width: 400px){
	form {display: block}
	input {
		width: 100%;
		display: block;
	}
	input[type="submit"]{
		width: 100%;
	}
	.images {
		column-count: 1;
	}
}
</style>
<form on:submit={getGifs}>
	<input id="search" type="text" style="index: 0" placeholder="Search your favourite gif images" bind:value={keyword} disabled={loading}>
	<input type="submit" value="Search" disabled={loading}/>
</form>
{#if gifs.length>0}
<div class="images">
	{#each gifs as gif}
		<img src={gif.images.fixed_height.url} alt="tag" width="200px" loading="lazy">
	{/each}
</div>
{:else if loading}
	<div style="width: 100%;text-align: center;">
		Loading
	</div>
{/if}