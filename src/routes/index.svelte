<script lang="ts">
  import { onMount, tick } from "svelte";

  interface GrassBlade {
    height: number;
  }

  let blades: GrassBlade[] = Array(10).fill(0).map((_, i) => ({
    height: Math.random() * 500 + 100
  }));

  let width = 600;
  let height = 600;

  let canvas: HTMLCanvasElement

  function draw() {
    const ctx = canvas.getContext("2d");
    if (!ctx) return;
    ctx.clearRect(0, 0, width, height);

    ctx.fillStyle = "green";
    blades.forEach((blade, i) => {
      ctx.fillRect((window.innerWidth / 11) + (i * window.innerWidth / 11), 0, 5, blade.height);
    });
  }

  onMount(() => {
    width = window.innerWidth;
    height = window.innerHeight;
    tick().then(draw)
  })
</script>
<svelte:window on:resize={() => {
  width = window.innerWidth;
  height = window.innerHeight;
  tick().then(draw)
}}></svelte:window>
<canvas bind:this={canvas} {width} {height}></canvas>