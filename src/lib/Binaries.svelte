<script lang="ts">
  import Binary, { type Cell } from "./Binary.svelte";

  const CELL_SIZE = {
    width: 20,
    height: 24,
  } as const;

  const calculateGrid = () => {
    return {
      rows: Math.floor(window.innerHeight / CELL_SIZE.height),
      cols: Math.floor(window.innerWidth / CELL_SIZE.width),
    };
  };

  let cells = $state<Cell[]>([]);
  let { rows, cols } = $state(calculateGrid());
  let total = $derived<number>(rows * cols);

  const resizeHandler = () => ({ rows, cols } = calculateGrid());

  const initializeCells = (): Cell[] => {
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

  $effect(() => {
    cells = initializeCells();
    const interval = setInterval(() => (cells = cells.map(randomizeCell)), 100);
    return () => clearInterval(interval);
  });
</script>

<svelte:window on:resize={resizeHandler} />

<div class="grid grid-cols-[repeat(auto-fill,minmax(20px,1fr))]">
  {#each cells as cell}
    <Binary {...cell} />
  {/each}
</div>
