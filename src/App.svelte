<script lang="ts">
  import { imageStatus } from './store';
  import { ImageStatus } from './types.d';
  import CloudinaryLogo from './components/CloudinaryLogo.svelte';
  import StepEdit from './components/StepEdit.svelte';
  import StepUpload from './components/StepUpload.svelte';
</script>

<div class="m-auto grid h-screen w-full max-w-xl grid-cols-1 place-content-center p-4">
  <header class="flex justify-center py-10">
    <h1 class="text-3xl font-bold tracking-tighter text-blue-900">
      Remove<span class="text-blue-600">bg</span>
    </h1>
  </header>

  <main>
    {#if $imageStatus === ImageStatus.READY || $imageStatus === ImageStatus.UPLOADING}
      <StepUpload />
    {:else if $imageStatus === ImageStatus.DONE}
      <StepEdit />
    {/if}
  </main>

  <footer class="flex items-center justify-center gap-x-2 pt-10 font-semibold">
    Made with
    <a href="https://cloudinary.com/" target="_blank" rel="noopener noreferrer">
      <CloudinaryLogo />
    </a>
  </footer>
</div>

<style>
</style>
