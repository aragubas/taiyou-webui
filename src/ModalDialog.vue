<script setup lang="ts">
import { onMounted, onUnmounted, reactive, ref, watch } from "vue";

const emit = defineEmits(["onClosed"]);
const props = defineProps({
  forceFocus: {
    type: Boolean,
    default: false,
    required: false,
  },
});

let hide = ref(false);
let closingAllowed = false;
let mouseInsideContent = false;
const forceFocusAnimation = ref(false);

function doClose(forceClose: boolean = false) {
  if (props.forceFocus && !forceClose) {
    // TODO: forceFocus animation
    forceFocusAnimation.value = true;
    setTimeout(() => {
      forceFocusAnimation.value = false;
    }, 0.25 * 1000);

    return;
  }

  hide.value = true;

  setTimeout(() => {
    emit("onClosed");
  }, 0.3 * 1000);
}

function closeDialog() {
  if (!closingAllowed || mouseInsideContent) {
    return;
  }

  doClose();
}

function keyUp(event: KeyboardEvent) {
  if (event.code == "Escape" && closingAllowed === true) {
    if (closingAllowed) {
      closeDialog();
    }
  }
}

onMounted(() => {
  document.addEventListener("keyup", keyUp);
  setTimeout(() => {
    closingAllowed = true;
  }, 0.4 * 1000);
});

onUnmounted(() => {
  document.removeEventListener("keyup", keyUp);
});

function requestClose(forceClose: boolean = false) {
  doClose(forceClose);
}

defineExpose({ requestClose });
</script>

<template>
  <div class="overlay main" @click="closeDialog" :class="[hide ? 'background-hide' : '']">
    <div
      @pointerenter="mouseInsideContent = true"
      @pointerleave="mouseInsideContent = false"
      :class="[hide ? 'foreground-hide' : '', forceFocusAnimation ? 'forcefocus-animation' : '']"
      class="foreground"
    >
      <slot></slot>
    </div>
  </div>
</template>

<style scoped>
@keyframes backgroundShow {
  from {
    background-color: transparent;
  }
  to {
    background-color: var(--background-overlay);
  }
}

@keyframes foregroundShow {
  from {
    transform: translateY(-105%);
  }

  to {
    transform: translateY(0%);
  }
}

div.main {
  animation: backgroundShow 0.3s var(--animation-presentation);
  transition: all 0.3s var(--animation-presentation);

  display: flex;
  justify-content: center;
  align-items: flex-start;
  background-color: var(--background-overlay);
}

.foreground {
  animation: foregroundShow 0.3s var(--animation-presentation);
  transition: all 0.3s var(--animation-presentation);

  background-color: var(--background-overlay-foreground);

  border-radius: 8px;
  border-top-left-radius: 0px;
  border-top-right-radius: 0px;
  box-shadow: var(--shadow-floating);
}

.foreground-hide {
  transform: translateY(-105%);
}

.background-hide {
  background-color: transparent !important;
}

.forcefocus-animation {
  will-change: transform;
  transform: scale(102%) translateY(1%);
  transition-duration: 0.15s;
  box-shadow: var(--shadow-floating-attention);
}
</style>
