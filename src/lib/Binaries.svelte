<script lang="ts">
  import Binary, { type Cell } from "./Binary.svelte";

  let cells = $state<Cell[]>([]);

  const initializeNumbers = () => {
    const columns = Math.floor(window.innerWidth / 20);
    const rows = Math.floor(window.innerHeight / 24);
    const total = columns * rows;

    cells = Array(total)
      .fill(0)
      .map(() => ({
        value: Math.round(Math.random()),
        opacity: Math.random(),
        delay: Math.random() * 0.5,
      }));
  };

  const updateNumbers = () => {
    cells = cells.map((n) => ({
      value: Math.random() > 0.7 ? Math.round(Math.random()) : n.value,
      opacity: Math.random() > 0.7 ? Math.round(Math.random()) : n.value,
      delay: n.delay,
    }));
  };

  $effect(() => {
    initializeNumbers();
    const interval = setInterval(updateNumbers, 100);
    return () => clearInterval(interval);
  });
</script>

<svelte:window on:resize={initializeNumbers} />

<div class="grid grid-cols-[repeat(auto-fill,minmax(20px,1fr))]">
  {#each cells as cell}
    <Binary {...cell} />
  {/each}
</div>
