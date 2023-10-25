<script lang="ts">
  import { onMount } from 'svelte';

  import { Cloudinary } from '@cloudinary/url-gen';
  import { backgroundRemoval } from '@cloudinary/url-gen/actions/effect';
  import Dropzone from 'dropzone';
  import 'dropzone/dist/dropzone.css';

  import { CLOUDINARY_API_KEY, CLOUDINARY_CLOUDNAME } from '@/consts';
  import { imageStatus, modifiedImage, originalImage } from '@/store';
  import { ImageStatus, type CloudinaryResponse } from '@/types.d';

  const cloudinary = new Cloudinary({
    cloud: { cloudName: CLOUDINARY_CLOUDNAME },
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
      formData.append('api_key', CLOUDINARY_API_KEY);
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
  class="box-border flex aspect-video h-[425px] w-full flex-col items-center justify-center gap-4 rounded-lg border-2 border-dashed border-gray-400 p-2 shadow-2xl"
  action={`https://api.cloudinary.com/v1_1/${CLOUDINARY_CLOUDNAME}/image/upload`}
>
  {#if $imageStatus === ImageStatus.READY}
    <button
      class="pointer-events-none rounded-xl bg-blue-800 px-8 py-4 text-xl font-semibold text-white"
    >
      Start from a photo
    </button>

    <strong class="text-lg text-gray-800">or drop a file</strong>
  {:else if $imageStatus === ImageStatus.UPLOADING}
    <strong class="text-lg text-gray-800"> Uploading...</strong>
  {/if}
</form>
