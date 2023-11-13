<script lang="ts">
  export let status = 'Success';
  export let message = 'Image uploaded successfully';
  export let color = 'bg-blue-600';
</script>

<div class="absolute bottom-6 left-6 z-10 box-border flex items-center justify-center text-white">
  <div
    class={`rectangle relative flex h-12 w-52 items-center justify-start overflow-hidden rounded-md ${color}`}
  >
    <div class="notification-text flex items-center px-4 py-0 text-base">
      <i class="bi bi-info-circle-fill" />
      <span>&nbsp;&nbsp;</span>
      <span>
        <b>{status}</b>
        {message}
      </span>
    </div>
  </div>
</div>

<style>
  .rectangle {
    animation:
      scale-in 0.3s ease-out forwards,
      expand 0.35s 0.25s ease-out forwards;
    transform: scale(0);
  }

  .notification-text {
    animation: fade-in 1s ease-in forwards;
  }

  @keyframes scale-in {
    100% {
      opacity: 1;
      transform: scale(1);
    }
  }

  @keyframes expand {
    50% {
      width: 400px;
    }

    100% {
      box-shadow:
        0px 1px 3px 0px rgba(0, 0, 0, 0.2),
        0px 1px 1px 0px rgba(0, 0, 0, 0.14),
        0px 3px 3px -1px rgba(0, 0, 0, 0.12);
      width: 400px;
    }
  }

  @keyframes fade-in {
    0% {
      opacity: 0;
    }

    100% {
      opacity: 0.8;
    }
  }
</style>
