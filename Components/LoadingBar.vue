<script setup lang="ts">
const props = defineProps({
  active: { type: Boolean, required: true },
  stretchHeight: { type: Boolean, required: false },
});
</script>

<template>
  <span class="loading-bar" :class="[active ? 'active' : '', stretchHeight ? 'stretch' : '']"></span>
</template>

<style scoped>
/* span.loading-bar.active {
  opacity: 1;
} */

span.loading-bar {
  display: block;
  position: relative;
  transition: opacity 0.5s ease;
  width: 100%;
  height: 4px;
  background-color: var(--loadingbar-background);
  overflow: hidden;
}

@keyframes loadingAnimation {
  from {
    transform: translateX(-100%);
  }
  to {
    transform: translateX(100%);
  }
}

span.loading-bar::after {
  content: "";
  position: absolute;
  animation: loadingAnimation 2s ease infinite;
  animation-play-state: paused;
  transition: opacity linear 0.5s;
  display: block;
  opacity: 0;
  width: 100%;
  height: 100%;
  background-color: var(--loadingbar-foreground);
}

span.loading-bar.active::after {
  opacity: 1;
  animation-play-state: running;
}

span.loading-bar.stretch {
  transition: height 0.25s ease;
  height: 0px;
}

span.loading-bar.active.stretch {
  height: 4px;
}
</style>
