<script>
	import {onMount} from 'svelte';
	onMount(() => {
		document.getElementById('search').focus();
	})
	let gifs = [];
	let keyword='';
	let loading = false;
	function getGifs(event){
		loading = true;
		gifs = [];
		event.preventDefault();
		fetch(`https://api.giphy.com/v1/gifs/search?api_key=O0PIjSQKNXlhQl4ro8ngi3paQujecP6C&q=${keyword}&limit=25&offset=0&rating=G&lang=en`)
		.then(res => res.json())
		.then(json => gifs = json.data);
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
<div class="images">
	{#each gifs as gif}
		<img autoplay src={gif.images.fixed_height.url} alt="tag" width="200px">
	{/each}
</div>