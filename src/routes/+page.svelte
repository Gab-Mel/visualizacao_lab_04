<svelte:head>
  <title>Gabmel: Personal site and portfolio</title>
</svelte:head>

<script>
    import projects from "$lib/projects.json";
    import Project from "$lib/Project.svelte";

    
    let profileData = fetch("https://api.github.com/users/Gab-Mel");
</script>
<h1>
    Gabriel 
</h1>
<p>
    Lorem ipsum dolor sit amet consectetur, adipisicing elit. Molestias nostrum corporis consequuntur dicta aspernatur voluptates magni veritatis maxime qui voluptatibus esse perspiciatis provident, explicabo eos suscipit sequi recusandae ea? Dolorum?
</p>
<img src="img/xinin.jpeg" alt="this is a robbery" width="300" height="350" >

{#await fetch("https://api.github.com/users/Gab-Mel")}
  <p>Loading...</p>
{:then response}
  {#await response.json()}
    <p>Decoding...</p>
  {:then data} 
    <section>
      <h2>My Github Stats</h2>
      <dl>
        <dt>Followers</dt>
        <dd>{data.followers}</dd>
        <dt>Following</dt>
        <dd>{data.following}</dd>
        <dt>Public Repos</dt>
        <dd>{data.public_repos}</dd>
      </dl>
    </section>
  {:catch error}
    <p class="error">Something went wrong: {error.message}</p>
  {/await}
  {:catch error}
    <p class="error">Something went wrong: {error.message}</p>
{/await}