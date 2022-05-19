<script lang="ts">
  import { onMount, tick } from "svelte";

  interface GrassBlade {
    xValues: number[];
    height: number;
  }

  const bladeAmount = 30;

  const generateRandomNumber = (min: number, max: number) => Math.random() * (max - min) + min;

  let blades: GrassBlade[] = Array(bladeAmount).fill(0).map((_, i) => {
    return {
      xValues: Array(10).fill(0).map(it => generateRandomNumber(-10, 10)),
      height: Math.random() * 500 + 100,
    }
  });

  let width = 600;
  let height = 600;

  let canvas: HTMLCanvasElement

  function draw() {
    const ctx = canvas.getContext("2d");
    if (!ctx) return;
    ctx.clearRect(0, 0, width, height);

    ctx.strokeStyle = "green";
    blades.forEach((blade, i) => {
      const x = (window.innerWidth / (bladeAmount + 1)) + (i * window.innerWidth / (bladeAmount + 1));
      ctx.beginPath();
      ctx.moveTo(x, 0);
      for (let i = 0; i < blade.xValues.length; i++) {
        ctx.lineTo(x + blade.xValues[i], blade.height / blade.xValues.length * i);
      }
      ctx.lineTo(x, blade.height);
      ctx.stroke();
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