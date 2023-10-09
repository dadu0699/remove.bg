<script lang="ts">
  import { onMount } from 'svelte';

  import { Cloudinary } from '@cloudinary/url-gen';
  import { backgroundRemoval } from '@cloudinary/url-gen/actions/effect';
  import Dropzone from 'dropzone';
  import 'dropzone/dist/dropzone.css';

  import { imageStatus, modifiedImage, originalImage } from '../store';
  import { ImageStatus, type CloudinaryResponse } from '../types.d';

  const cloudinary = new Cloudinary({
    cloud: { cloudName: 'dzchmybac' },
    url: { secure: true },
  });

  onMount(() => {
    const dropzone = new Dropzone('#dropzone', {
      uploadMultiple: false,
      maxFiles: 1,
      acceptedFiles: '.jpg,.jpeg,.png,.webp',
    });

    dropzone.on('sending', (file, xhr, formData) => {
      imageStatus.set(ImageStatus.UPLOADING);

      formData.append('upload_preset', 'ukqsp4dy');
      formData.append('timestamp', String(Date.now() / 1000));
      formData.append('api_key', '');
    });

    dropzone.on('success', (file, response) => {
      const { public_id: publicId, secure_url: url } = response as CloudinaryResponse;

      imageStatus.set(ImageStatus.DONE);
      originalImage.set(url);

      const imageWhitoutBackground = cloudinary.image(publicId).effect(backgroundRemoval());
      modifiedImage.set(imageWhitoutBackground.toURL());
    });

    dropzone.on('error', (file, response) => {
      console.log('error', response);
    });
  });
</script>

<form
  id="dropzone"
  class="flex aspect-video h-[520px] w-full flex-col items-center justify-center rounded-lg border-2 border-dashed border-gray-300 shadow-2xl"
  action="https://api.cloudinary.com/v1_1/dzchmybac/image/upload"
>
  {#if $imageStatus === ImageStatus.READY}
    <button
      class="pointer-events-none rounded-full bg-blue-600 px-8 py-4 text-xl font-bold text-white"
    >
      Upload image
    </button>
    <strong class="mt-4 text-lg text-gray-800">or drop a file</strong>
  {:else if $imageStatus === ImageStatus.UPLOADING}
    <strong class="mt-4 text-lg text-gray-800"> Uploading...</strong>
  {/if}
</form>

<button
  class="mt-10 block w-full rounded-full bg-blue-600 px-4 py-2 text-center text-xl font-bold text-white hover:bg-blue-700 disabled:cursor-not-allowed disabled:opacity-50"
  disabled
>
  Download image without background
</button>
