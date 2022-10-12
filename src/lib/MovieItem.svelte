<script>
  export let title = '';
  export let imdbId = '';

  const getRatings = async () => {
    const url = import.meta.env.VITE_PUBLIC_OMDB_API_URL;
    const options = { method: 'GET' };
    return fetch(
      url +
        new URLSearchParams({
          apikey: import.meta.env.VITE_PUBLIC_OMDB_API_KEY,
          i: imdbId
        }),
      options
    )
      .then((response) => response.json())
      .then((response) => {
        return response.Ratings;
      })
      .catch((err) => console.error(err));
  };
</script>

<p>
  <a
    href={`https://www.imdb.com/title/${imdbId}`}
    target="_blank"
    class="hover:text-neutral-500 transition-colors"
  >
    {title}
  </a>
  {#await getRatings()}
    <p>loading...</p>
  {:then ratings}
    {#each ratings as rating}
      <p>
        {#if rating.Source === 'Internet Movie Database'}
          imdb:
        {:else if rating.Source === 'Rotten Tomatoes'}
          RT:
        {:else if rating.Source === 'Metacritic'}
          metacritic:
        {/if}
        {rating.Value}
      </p>
    {/each}
  {:catch error}
    <p>couldn't load ratings...</p>
  {/await}
</p>
