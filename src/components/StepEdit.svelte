<script lang="ts">
  import 'two-up-element';

  import { modifiedImage, originalImage } from '@/store';
  import Loader from './Loader.svelte';
  import type { Notification } from '@/types';

  export let showNotification: ({ status, message, color }: Notification) => Promise<void>;
  let intervalId: number;
  let processingImage = true;
  let tries = 0;
  let success = false;

  $: {
    if (processingImage) {
      clearInterval(intervalId);
      intervalId = setInterval(() => {
        tries++;

        if (tries > 10) {
          clearInterval(intervalId);
          processingImage = false;
          success = false;
          showNotification({
            message: 'Something went wrong. Please try again later.',
            color: 'bg-red-600',
          });
          return;
        }

        const img = new Image();
        img.src = $modifiedImage;
        img.onload = () => {
          processingImage = false;
          success = true;
          clearInterval(intervalId);
        };
      }, 1000);
    }
  }
</script>

<div
  class="relative box-border flex h-[425px] items-center justify-center rounded-lg border-2 border-dashed border-gray-400 p-2 shadow-2xl"
>
  {#if processingImage}
    <Loader />
  {:else}
    <two-up>
      <img
        src={$originalImage}
        alt="Original uploaded by user"
        class="aspect-video h-96 w-full object-contain"
        height={300}
        width={300}
      />

      {#if !success}
        <img
          src={$originalImage}
          alt="Original uploaded by user with filter applied"
          class="aspect-video h-96 w-full object-contain blur-sm filter"
          height={300}
          width={300}
        />
      {:else}
        <img
          src={$modifiedImage}
          alt="Without background uploaded by user"
          class="aspect-video h-96 w-full object-contain"
          height={300}
          width={300}
        />
      {/if}
    </two-up>
  {/if}

  {#if !processingImage && success}
    <div class="absolute right-0 top-0 flex flex-col p-2.5">
      <a
        class="inline-flex items-center rounded-full bg-blue-800 p-2.5 text-center text-sm font-medium text-white hover:bg-blue-700"
        href={$modifiedImage || $originalImage}
        rel="noopener noreferrer"
        target="_blank"
      >
        <svg
          class="h-4 w-4 fill-current"
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 20 20"
          stroke="currentColor"
          fill="none"
        >
          <path d="M13 8V2H7v6H2l8 8 8-8h-5zM0 18h20v2H0v-2z" />
        </svg>
        <span class="sr-only">Download image without background</span>
      </a>
    </div>
  {/if}
</div>
