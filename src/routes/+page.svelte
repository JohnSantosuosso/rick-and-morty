<script>
  import { graphql } from '$houdini';

  const randomStore = graphql`
      query GenerateRandomCharacter($id: ID!) {
          character(id: $id) {
              name
              image
              status
              species
              gender
          }
      }
  `;

  function getRandomCharacter() {
    // Generate random ID between 1 and 873
    const randomId = Math.floor(Math.random() * 873) + 1;
    randomStore.fetch({ variables: { id: randomId.toString() } });
  }
</script>

<div class="container mx-auto p-4">
  <h1 class="text-3xl font-bold mb-6" style="text-align: center;">Rick and Morty Characters</h1>
  
  <!-- Random Character Section -->
  <div class="mb-8" style="text-align: center;">
    <button
      on:click={getRandomCharacter}
      class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded mb-4"
    >
      Get Random Character
    </button>
    <br>
    <br>
    <br>
  <div class="container mx-auto p-4 mt-4">
    {#if $randomStore.loading}
      <div>Loading random character...</div>
    {:else if $randomStore.error}
      <div class="text-red-500">Error: {$randomStore.error.message}</div>
    {:else if $randomStore.data?.character}
      <div class="border rounded-lg shadow-lg overflow-hidden max-w-sm">
        <img
          src={$randomStore.data.character.image}
          alt={$randomStore.data.character.name}
          class="w-full h-48 object-cover"
        />
        <div class="flex justify-center">
          <div class="p-4 text-left">
            <h2 class="text-xl font-semibold mb-2">{$randomStore.data.character.name}</h2>
            <p class="mb-1">Status: {$randomStore.data.character.status}</p>
            <p class="mb-1">Species: {$randomStore.data.character.species}</p>
            <p class="mb-1">Gender: {$randomStore.data.character.gender}</p>
          </div>
        </div>
      </div>
    {/if}
    </div>
  </div>
</div>