<script>
  import { graphql } from '$houdini';

  const store = graphql`
      query CharactersList {
          characters {
              info {
                  count
                  pages
                  next
                  prev
              }
              results {
                  id
                  name
                  status
                  species 
                  gender
                  image
              }
          }
      }
  `;
</script>

<div class="container mx-auto p-4">
  <h1 class="text-3xl font-bold mb-6">Rick and Morty Characters</h1>

  {#if $store.loading}
      <div>Loading...</div>
  {:else if $store.error}
      <div class="text-red-500">Error: {$store.error.message}</div>
  {:else if $store.data}
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
          {#each $store.data.characters.results as character}
              <div class="border rounded-lg shadow-lg overflow-hidden">
                  <img
                      src={character.image}
                      alt={character.name}
                      class="w-full h-48 object-cover"
                  />
                  <div class="p-4">
                      <h2 class="text-xl font-semibold mb-2">{character.name}</h2>
                      <p class="mb-1">Status: {character.status}</p>
                      <p class="mb-1">Species: {character.species}</p>
                      <p class="mb-1">Gender: {character.gender}</p>
                  </div>
              </div>
          {/each}
      </div>
  {/if}
</div>