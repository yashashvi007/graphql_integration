<script context="module">
  import gql from 'graphql-tag';
  import { client } from './apollo';

  const ARTICLES = gql`
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
    console.log(ARTICLES)
    const {data} = await client.query({query: ARTICLES})
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
  restore(client, ARTICLES, cache);

  const articles = query(client, { query: ARTICLES });
  console.log('asdas', articles)
</script>

<ul>
  {#await $articles}
    <li>Loading...</li>
  {:then result}
    {#each result.data.Document as article (article.id)}
      <li>{article.name}</li>
    {:else}
      <li>No articles found</li>
    {/each}
  {:catch error}
    <li>Error loading articles: {error}</li>
  {/await}
</ul>