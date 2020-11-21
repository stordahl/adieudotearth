<script>
	import Map from './components/Map.svelte';
	import Loading from './components/Loading.svelte';
	export let ready;
</script>

<svelte:head>
	<script defer async
	src="https://maps.googleapis.com/maps/api/js?key={KEY}&callback=initMap">
	</script>
</svelte:head>

<style>
	:global(body) {
		padding: 0;
	}
	h1 {
		position: absolute;
		top: -10vh;
		left: 10vw;
	}
	footer {
		width: 100%;
		text-align: center;
	}
	@media screen and (max-width:768px){
		h1 {
			top: -10vh;
			left: 5vw;
		}
	}
</style>

<h1>adieu.earth</h1>

{ #if ready }
<Map></Map>
{:else}
<Loading />
{ /if }

<footer>
	<h5>built by <a href="https://stordahl.dev">Jacob Stordahl</a> | follow me on <a href="https://twitter.com/stordahl_">Twitter</a></h5>
</footer>