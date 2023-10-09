<script lang="ts">
  import 'two-up-element';

  import { modifiedImage, originalImage } from '../store';

  let intervalId: number;
  let processingImage = true;
  let tries = 0;

  $: {
    if (processingImage) {
      clearInterval(intervalId);
      intervalId = setInterval(() => {
        tries++;

        const img = new Image();
        img.src = $modifiedImage;
        img.onload = () => {
          processingImage = false;
          clearInterval(intervalId);
        };
      }, 500);
    }
  }
</script>

<two-up class="border-2 border-dashed border-gray-300 p-2 shadow-2xl">
  <img
    src={$originalImage}
    alt="Original uploaded by user"
    class="aspect-square h-[500px] max-h-[500px] w-full object-contain"
    height={200}
    width={200}
  />

  {#if processingImage}
    <div class="flex flex-col items-center justify-center">
      <p class="mt-4 text-center">Processing image...</p>
    </div>
  {:else}
    <img
      src={$modifiedImage}
      alt="Without background uploaded by user"
      class="aspect-square h-[500px] max-h-[500px] w-full object-contain"
      height={200}
      width={200}
    />
  {/if}
</two-up>

<a
  download
  href={$modifiedImage}
  class="mt-10 block w-full rounded-full bg-blue-600 px-4 py-2 text-center text-xl font-bold text-white hover:bg-blue-700"
>
  Download image without background
</a>
