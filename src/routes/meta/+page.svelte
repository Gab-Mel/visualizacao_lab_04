<svelte:head>
  <title>Gabmel: Personal site and portfolio</title>
</svelte:head>

<script>
    import * as d3 from "d3";

    import { onMount } from "svelte";

    let data = [];
    let commits = [];

    onMount(async () => {
      data = await d3.csv("./loc.csv", row => ({
    ...row,
    line: Number(row.line), // or just +row.line
    depth: Number(row.depth),
    length: Number(row.length),
    date: new Date(row.date + "T00:00" + row.timezone),
    datetime: new Date(row.datetime)
}));

commits = d3.groups(data, d => d.commit).map(([commit, lines]) => {
    let first = lines[0];
    let {author, date, time, timezone, datetime} = first;
    let ret = {
        id: commit,
        url: "https://github.com/Gab-Mel/visualizacao_lab_04/commit/" + commit,
        author, date, time, timezone, datetime,
        hourFrac: datetime.getHours() + datetime.getMinutes() / 60,
        totalLines: lines.length
    };

    // Like ret.lines = lines
    // but non-enumerable so it doesn’t show up in JSON.stringify
    Object.defineProperty(ret, "lines", {
        value: lines,
        configurable: true,
        writable: true,
        enumerable: false,
    });

    return ret;
});
    console.log(commits)
});



let width = 1000, height = 600;

$: minDate = d3.min(commits.map(d => d.date));
$: maxDate = d3.max(commits.map(d => d.date));
console.log(commits.map(d => d.date))
$: maxDatePlusOne = new Date(maxDate);
$: maxDatePlusOne.setDate(maxDatePlusOne.getDate() + 1);
$: xScale = d3.scaleTime()
              .domain([minDate, maxDatePlusOne])
              .range([0, width])
              .nice();

$: yScale = d3.scaleLinear()
              .domain([24, 0])
              .range([height, 0]);

let margin = {top: 10, right: 10, bottom: 30, left: 20};

let usableArea = {
    top: margin.top,
    right: width - margin.right,
    bottom: height - margin.bottom,
    left: margin.left
};
usableArea.width = usableArea.right - usableArea.left;
usableArea.height = usableArea.bottom - usableArea.top;

let xAxis, yAxis;


$: {
    d3.select(xAxis).call(d3.axisBottom(xScale));
    d3.select(yAxis).call(d3.axisLeft(yScale));
}
</script>



<svg viewBox="0 0 {width} {height}">
  <g class="dots">
    {#each commits as commit, index }
        <circle
            cx={ xScale(commit.datetime) }
            cy={ yScale(commit.hourFrac) }
            r="5"
            fill="steelblue"
        />
    {/each}
    </g>
    <g transform="translate(0, {usableArea.bottom})" bind:this={xAxis} />
<g transform="translate({usableArea.left}, 0)" bind:this={yAxis} /> 
</svg>

<section>
  <h2>Summary</h2>
  <dl class="stats">
  <dt>Total <abbr title="Lines of code">LOC</abbr></dt>
  <dd>{data.length}</dd>
  <dt>Files</dt>
  <dd>{d3.groups(data, d => d.file).length}</dd>
  <dt>Commits</dt>
  <dd>{d3.groups(data, d => d.commit).length}</dd>
  </dl>
</section>


<style>
  svg {
      overflow: visible;
  }
</style>
