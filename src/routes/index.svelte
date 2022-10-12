<script>
  import MovieItem from '$lib/MovieItem.svelte';

  const getMovies = async () => {
    const url = import.meta.env.VITE_PUBLIC_PLEX_API_URL;
    const options = { method: 'GET', headers: { Accept: 'application/json' } };
    return fetch(
      url +
        new URLSearchParams({
          'X-Plex-Token': import.meta.env.VITE_PUBLIC_PLEX_TOKEN,
          // year: '2022',
          sort: 'addedAt:desc',
          limit: '10'
        }),
      options
    )
      .then((response) => response.json())
      .then((response) => {
        return response.MediaContainer.Metadata;
      })
      .catch((err) => console.error(err));
  };
</script>

{#await getMovies()}
  <p>loading...</p>
{:then movies}
  {#each movies as movie}
    <MovieItem
      title={movie.title}
      imdbId={movie.Media[0].Part[0].file.split('/')[3].split(' ').pop().slice(1, -1)}
    />
  {/each}
{:catch error}
  <p>couldn't load movies...</p>
{/await}
