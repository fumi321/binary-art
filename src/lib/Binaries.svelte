<script lang="ts">
  import Binary, { type Cell } from "./Binary.svelte";

  let cells = $state<Cell[]>([]);

  const initializeCells = (): Cell[] => {
    const columns = Math.floor(window.innerWidth / 20);
    const rows = Math.floor(window.innerHeight / 24);
    const total = columns * rows;

    return Array(total)
      .fill(0)
      .map(() => ({
        value: Math.round(Math.random()),
        opacity: Math.random(),
        delay: Math.random(),
      }));
  };

  const randomizeCell = (cell: Cell): Cell => {
    if (Math.random() <= 0.7) {
      return cell;
    }
    return {
      value: Math.round(Math.random()),
      opacity: Math.random(),
      delay: cell.delay,
    };
  };

  const updateCells = (cells: Cell[]): Cell[] => {
    return cells.map((cell) => randomizeCell(cell));
  };

  $effect(() => {
    cells = initializeCells();
    const interval = setInterval(() => (cells = updateCells(cells)), 100);
    return () => clearInterval(interval);
  });
</script>

<svelte:window on:resize={initializeCells} />

<div class="grid grid-cols-[repeat(auto-fill,minmax(20px,1fr))]">
  {#each cells as cell}
    <Binary {...cell} />
  {/each}
</div>
