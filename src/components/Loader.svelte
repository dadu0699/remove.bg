<script lang="ts"></script>

<div class="sound-wave">
  <div></div>
  <div></div>
  <div></div>
  <div></div>
  <div></div>
</div>

<style>
  @keyframes audio-wave {
    0% {
      background: #ff8e3a;
      height: 6px;
      transform: translateY(0px);
    }

    25% {
      background: #ed509e;
      height: 40px;
      transform: translateY(-5px) scaleY(1.7);
    }

    50% {
      background: #9c73f8;
      height: 6px;
      transform: translateY(0px);
    }

    100% {
      background: #0fccce;
      height: 6px;
      transform: translateY(0px);
    }
  }

  .sound-wave {
    align-items: center;
    display: flex;
    gap: 8px;
    height: 40px;
    justify-content: space-between;
    width: 60px;
  }

  .sound-wave div {
    animation: audio-wave 2.2s infinite ease-in-out;
    background: orange;
    border-radius: 8px;
    display: block;
    height: 6px;
    width: 10px;
  }

  .sound-wave div:nth-child(2) {
    left: 11px;
    animation-delay: 0.2s;
  }

  .sound-wave div:nth-child(3) {
    left: 22px;
    animation-delay: 0.4s;
  }

  .sound-wave div:nth-child(4) {
    left: 33px;
    animation-delay: 0.6s;
  }

  .sound-wave div:nth-child(5) {
    left: 44px;
    animation-delay: 0.8s;
  }
</style>
