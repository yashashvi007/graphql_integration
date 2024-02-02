<script context="module">
  import gql from 'graphql-tag';
  import { client } from './apollo';

  const DOCUMENTS = gql`
  query MyQuery @cached {
      Document {
        description
        id
        name
        views
      }
    }
  `;
  export async function preload() {
    console.log(DOCUMENTS)
    const {data} = await client.query({query: DOCUMENTS})
    console.log(data)
    return {
      cache: data
    };
  }
</script>

<script>
  import { restore, query } from 'svelte-apollo';
  
  export let cache;
  console.log('sadasdsa',cache)
  restore(client, DOCUMENTS, cache);

  const docs = query(client, { query: DOCUMENTS });
  console.log('asdas', docs)
</script>

<ul>
  {#await $docs}
    <li>Loading...</li>
  {:then result}
    {#each result.data.Document as doc (doc.id)}
      <li>{doc.name}</li>
    {:else}
      <li>No docs found</li>
    {/each}
  {:catch error}
    <li>Error loading docs: {error}</li>
  {/await}
</ul>