<script lang="ts">
  import { tweened } from "svelte/motion";
  import { cubicOut, expoOut } from "svelte/easing";
  import { writable } from "svelte/store";
  import { onMount, onDestroy } from "svelte";

  import { Minimize2 } from "lucide-svelte";

  let n = tweened(40, {
    duration: 100,
    easing: expoOut,
    interpolate: (a, b) => (t) => Math.round(a + (b - a) * t),
  });
  let progress = tweened(0, {
    duration: 1000,
    easing: cubicOut,
  });

  let showProgress = writable(true);
  let showControls = writable(true);
  let showBoxShadow = writable(true);
  let makeNonGreenRed = writable(false);

  let shadowBlur = writable(20);
  let shadowSpread = writable(1);
  let shadowColor = writable("#50b93b");
  let redShadowColor = writable("#ff0000");

  // Simulate progress update
  setTimeout(() => {
    progress.set(35);
  }, 1000);

  const handleKeydown = (event: any) => {
    if (event.key === "k") {
      showControls.update((value) => !value);
    }
  };

  onMount(() => {
    if (typeof window !== "undefined") {
      window.addEventListener("keydown", handleKeydown);
    }
  });

  onDestroy(() => {
    if (typeof window !== "undefined") {
      window.removeEventListener("keydown", handleKeydown);
    }
  });
</script>

<link rel="preconnect" href="https://fonts.googleapis.com" />
<link
  rel="preconnect"
  href="https://fonts.gstatic.com"
  crossorigin="anonymous"
/>
<link
  href="https://fonts.googleapis.com/css2?family=Inter:ital,opsz,wght@0,14..32,100..900;1,14..32,100..900&display=swap"
  rel="stylesheet"
/>

<main class="flex flex-col gap-10">
  <div class="flex flex-col w-1/4">
    <h1 class="text-2xl mb-4">Dashed Vertical Fill</h1>
    {#if $showProgress}
      <div
        class="progress flex flex-row justify-between text-sm text-gray-500 mb-2"
      >
        <h3>0%</h3>
        <h3>50%</h3>
        <h3>100%</h3>
      </div>
    {/if}
    <div class="bar flex justify-between lines mb-4">
      {#each Array.from({ length: $n }) as _, i}
        <div
          class="line border rounded-full h-8"
          style="background-color: {i < ($progress / 100) * $n
            ? '#50b93b'
            : $makeNonGreenRed
              ? 'red'
              : 'white'}; border: none; box-shadow: {$showBoxShadow &&
          i < ($progress / 100) * $n
            ? `0 0 ${$shadowBlur}px ${$shadowSpread}px ${$shadowColor}`
            : $showBoxShadow && $makeNonGreenRed
              ? `0 0 ${$shadowBlur}px ${$shadowSpread}px ${$redShadowColor}`
              : 'none'};"
        ></div>
      {/each}
    </div>
    {#if $showControls}
      <div class="flex flex-row justify-between text-sm text-gray-500 mb-2">
        <label>
          <input type="checkbox" bind:checked={$showProgress} />
          Show Progress
        </label>
      </div>

      <div class="flex flex-row justify-between text-sm text-gray-500 mb-2">
        <label>
          <input type="checkbox" bind:checked={$showBoxShadow} />
          Show Box Shadow
        </label>
      </div>

      <div class="flex flex-row justify-between text-sm text-gray-500 mb-2">
        <label>
          <input type="checkbox" bind:checked={$makeNonGreenRed} />
          Make Non-Green Red
        </label>
      </div>

      <div class="mb-2">
        <label for="n-range">Number of Divs:</label>
        <input id="n-range" type="range" min="10" max="70" bind:value={$n} />
      </div>

      <div class="mb-2">
        <label for="n-input">Number of Divs:</label>
        <input
          id="n-input"
          type="number"
          min="10"
          max="70"
          bind:value={$n}
          class="text-black"
        />
      </div>

      <div class="mb-2">
        <label for="progress-range">Progress:</label>
        <input
          id="progress-range"
          type="range"
          min="0"
          max="100"
          bind:value={$progress}
        />
      </div>

      <div class="mb-2">
        <label for="shadow-blur">Shadow Blur:</label>
        <input
          id="shadow-blur"
          type="range"
          min="0"
          max="150"
          bind:value={$shadowBlur}
        />
      </div>

      <div class="mb-2">
        <label for="shadow-spread">Shadow Spread:</label>
        <input
          id="shadow-spread"
          type="range"
          min="0"
          max="10"
          bind:value={$shadowSpread}
        />
      </div>

      <div class="mb-2">
        <label for="shadow-color">Shadow Color:</label>
        <input id="shadow-color" type="color" bind:value={$shadowColor} />
      </div>

      <div class="mb-2">
        <label for="red-shadow-color">Red Shadow Color:</label>
        <input
          id="red-shadow-color"
          type="color"
          bind:value={$redShadowColor}
        />
      </div>
    {/if}
  </div>
  <div class="bg-zinc-950 w-1/4 h-60">
    <span class="font-bold">$21,235.88</span>
    <span
      ><Minimize2 />
      min 7.4%</span
    >
    <span>database icon earned +356.35</span>
    <span>Unlock at</span>
    <span>August 12, 2024</span> <span>35% maturity lockicon</span>
  </div>
</main>

<style>
  main {
    font-family: "Inter", sans-serif;
    display: flex;
    height: 100vh;
    background-color: black;
    justify-content: center;
    align-items: center;
    color: white;
  }
  .line {
    width: 4px;
  }
  input[type="range"],
  input[type="number"],
  input[type="color"] {
    margin-top: 10px;
  }
</style>
