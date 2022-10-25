<script>
  import sanityClient from "@sanity/client";
  import { PortableText } from "@portabletext/svelte";

  const client = sanityClient({
    projectId: "w8m31798",
    dataset: "production",
    useCdn: false,
    apiVersion: "2021-10-21",
  });

  const fetchAuthors = (async() => {
    const response = await client.fetch('*[_type == "author"]');
    if (response) {
      return {
        status: 200,
        body: {
          authors: response,
        },
      };
    }
    return {
      status: 500,
      body: new Error("Internal Server Error"),
    };
  })()

  console.log(fetchAuthors)
  
</script>

<body>
{#await fetchAuthors}
  <p>loading...</p>
{:then authors}
  <ul>
    {#each authors.body.authors as author}
      <li>{author.name}</li>
      <PortableText value={author.bio} />
    {/each}
  </ul>
{:catch error}
  <p>{error.message}</p>
{/await}

</body>