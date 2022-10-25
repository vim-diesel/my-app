<script>
  import sanityClient from "@sanity/client";
  //import { PortableText } from "@portabletext/svelte";

  const client = sanityClient({
    projectId: "w8m31798",
    dataset: "production",
    useCdn: false,
    apiVersion: "2021-10-21",
  });

  const fetchAuthors = client.fetch('*[_type == "post"]');
  
</script>

<main>
{#await fetchAuthors}
  <p>loading...</p>
{:then postList}
  <ul>
    {#each postList as post}
      <li>{post.name}</li>
      <!-- <PortableText value={author.bio} /> -->
    {/each}
  </ul>
{:catch error}
  <p>{error.message}</p>
{/await}

</main>