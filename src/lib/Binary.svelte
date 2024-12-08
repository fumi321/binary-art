<script lang="ts">
  export type Cell = {
    value: number;
    opacity: number;
    delay: number;
  };

  let { value, opacity, delay } = $props<Cell>();
</script>

<div
  class="text-center transition-all duration-500 select-none font-mono"
  style="color: rgba(0, 255, 0, {opacity}); transition-delay: {delay}s"
>
  {value}
</div>
