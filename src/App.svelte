<script>
	import ApolloClient from 'apollo-client';
	import { client } from './apollo';
	import { setClient } from 'svelte-apollo';
	import Docs, { preload as docsPreload } from './Docs.svelte';

	const docsPreloading =  docsPreload();
	setClient(client);
</script>

<style>

</style>

<section>
	<h2 class="text-red">Docs  (simple query)</h2>

	{#await docsPreloading}
		<p>Preloading Docs....</p>
	{:then preloaded}
		<Docs {...preloaded} />
	{:catch error}
		<p>Error preloading Docs: {error}</p>
	{/await}
</section>
