<script lang="ts">
  import { browser } from "$app/environment";

  let show = false;
  let time = 0.5;
  let done = false;

  // only show the button once the content has fully loaded
  let ready = false;
  if (browser) {
    window.addEventListener(
      "load",
      (event) => {
        ready = true;
      },
      { once: true }
    );
  }
</script>

{#if show}
  <slot />
{:else}
  <div class="flex items-center justify-center w-screen h-screen">
    <div>
      <h1 class="text-xl font-bold">flash test</h1>
      {#if !done}
        <span
          >you'll see a prototype, image, or something else appear on screen for</span
        >
        <br />
        <bold class="font-bold">{time} seconds</bold>.
        <p class="mb-8 border-b-2 border-gray-200">
          don't continue without more instructions
        </p>
        <label class="mt-8">
          <input
            bind:value={time}
            class="bg-gray-100 w-24"
            type="number"
            min="0.2"
            max="10"
            step="0.05"
          />
          <span>sec</span>
        </label>
        {#if ready}
          <button
            class="border-gray-700 underline ms-4 text-blue-500 hover:text-blue-900"
            on:click={() => {
              show = true;
              setTimeout(() => {
                show = false;
                done = true;
              }, time * 1000);
            }}>click when asked</button
          >
        {:else}
          <span class="ms-4">loading...</span>
        {/if}

        <!-- render a dummy copy offscreen so the browser keeps what we are going to flash cached and visible instantly -->
        <div class="opacity-0 absolute top-full left-full" aria-hidden="true">
          <slot />
        </div>
      {:else}
        <span>expect questions now, you can close this tab.</span>
      {/if}
    </div>
  </div>
{/if}
