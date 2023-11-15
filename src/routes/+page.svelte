<script lang="ts">
  import { onMount } from 'svelte';
  import '../style.css';

  const sleep = (ms: number) =>
    new Promise((resolve) => setTimeout(resolve, ms));

  let text = 'Greetings, Traveler.';
  let state: 'streaming' | 'decrypting' | 'done' = 'done';
  let liveText: { char: string; font: string }[] = [];
  let fonts = ['argon', 'krypton', 'neon', 'radon', 'xenon'];

  function randomChar(): string {
    const chars =
      'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890';
    return chars[Math.floor(Math.random() * chars.length)];
  }

  function randomFont() {
    return fonts[Math.floor(Math.random() * fonts.length)];
  }

  async function startStreaming() {
    state = 'streaming';
    liveText = [];

    for (let i in text.split('')) {
      if (text[i] === ' ') {
        liveText = [...liveText, { char: ' ', font: '' }];
      } else {
        liveText = [...liveText, { char: randomChar(), font: randomFont() }];
      }

      await sleep(40);
    }

    state = 'decrypting';
    startDecrypting();
  }

  async function startDecrypting() {
    for (let index in liveText) {
      liveText[index] = { char: text[index], font: randomFont() };
      await sleep(60);
    }

    state = 'done';
  }

  onMount(() => {
    startStreaming();
  });
</script>

<main class="p-6 text-2xl">
  <p>
    {#each liveText as { char, font }, i (i)}
      {#if char === ' '}
        <span>&nbsp;</span>
      {:else}
        <span class={font}>{char}</span>
      {/if}
    {/each}
  </p>
</main>
