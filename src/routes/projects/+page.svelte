<svelte:head>
  <title>My projects</title>
</svelte:head>
<script>
    import projects from "$lib/projects.json";
    import Project from "$lib/Project.svelte";
    import Pie from "../../lib/Pie.svelte";
    let pieData = [
    { value: 1, label: "apples" },
    { value: 2, label: "oranges" },
    { value: 3, label: "mangos" },
    { value: 4, label: "pears" },
    { value: 5, label: "limes" },
    { value: 5, label: "cherries" }
];
</script>

<!-- <pre>{ JSON.stringify(projects, null, "\t") }</pre>   -->

<h1>{ projects.length} Projects</h1>
<div><Pie data={pieData} /></div>

<div class="projects">
    
    {#each projects as p}
        <Project data={p}/>
      <!-- <article>
        <h2>{p.title}</h2>
        <img src={p.image} alt="" />
        <p>
            {p.description}
        </p>
      </article> -->
    {/each}
</div>