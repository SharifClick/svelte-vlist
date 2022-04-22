<script>
  import VirtualList from "../src/index.js";
  import TargetComponent from "./helpers/TargetComponent.svelte";
  import data from "./helpers/data.json";
  const sleep = (ms) => new Promise((resolve) => setTimeout(resolve, ms));

  let records = data.records;
  let height = 600,
    heights = [800, 600, 500, 300, 100],
    loading = false;
  export let _items = new Array(100)
    .fill()
    .map(() => records[Math.floor(Math.random() * records.length)]);

  export let start = 0,
    end = 0;

  $: {
    if (end === _items.length - 10) {
      loadMore();
    }
  }

  const loadMore = async () => {
    loading = true;
    await sleep(1000);
    let itemsToAdd = _items.sort(() => 0.5 - Math.random()).slice(0, 20); // get random 20 items
    _items = [..._items, ...itemsToAdd];
    loading = false;
  };

  function rowClicked(e) {
    console.log(e.detail);
  }
</script>






<a
  href="https://github.com/SharifClick/svelte-vlist"
  class="github-corner"
  aria-label="View source on GitHub">
  <svg
    width="80"
    height="80"
    viewBox="0 0 250 250"
    style="fill:#151513; color:#fff; position: absolute; top: 0; border: 0;
    right: 0;"
    aria-hidden="true">
    <path d="M0,0 L115,115 L130,115 L142,142 L250,250 L250,0 Z" />
    <path
      d="M128.3,109.0 C113.8,99.7 119.0,89.6 119.0,89.6 C122.0,82.7 120.5,78.6
      120.5,78.6 C119.2,72.0 123.4,76.3 123.4,76.3 C127.3,80.9 125.5,87.3
      125.5,87.3 C122.9,97.6 130.6,101.9 134.4,103.2"
      fill="currentColor"
      style="transform-origin: 130px 106px;"
      class="octo-arm" />
    <path
      d="M115.0,115.0 C114.9,115.1 118.7,116.5 119.8,115.4 L133.7,101.6
      C136.9,99.2 139.9,98.4 142.2,98.6 C133.8,88.0 127.5,74.4 143.8,58.0
      C148.5,53.4 154.0,51.2 159.7,51.0 C160.3,49.4 163.2,43.6 171.4,40.1
      C171.4,40.1 176.1,42.5 178.8,56.2 C183.1,58.6 187.2,61.8 190.9,65.4
      C194.5,69.0 197.7,73.2 200.1,77.6 C213.8,80.2 216.3,84.9 216.3,84.9
      C212.7,93.1 206.9,96.0 205.4,96.6 C205.1,102.4 203.0,107.8 198.3,112.5
      C181.9,128.9 168.3,122.5 157.7,114.1 C157.9,116.9 156.7,120.9 152.7,124.9
      L141.0,136.5 C139.8,137.7 141.6,141.9 141.8,141.8 Z"
      fill="currentColor"
      class="octo-body" />
  </svg>
</a>
<div class="container">
  <div class="row">
    <div class="column is-narrow">
      <p class="has-text-centered">
        Showing items {start}-{end} of {_items.length}
      </p>
      <div
        style="height:{height}px;width:500px; border:1px solid rgba(0,0,0,0.1); position:relative;">
        <VirtualList items={_items} bind:start bind:end let:item let:index>
          <TargetComponent {item} {index} on:rowClick={rowClicked} />
        </VirtualList>
      </div>
    </div>
  </div>
</div>
