<script setup lang="ts">
import { onUnmounted, ref } from "vue";
import { useToastNotification } from "../Stores/ToastNotificationStore";

const toastNotification = useToastNotification();
</script>

<template>
  <div class="toast-viewport">
    <TransitionGroup tag="ol" name="fade" class="flex-column-reverse toast-list">
      <li
        v-for="(toast, id) in toastNotification.toastNotifications"
        class="toast-notification"
        @click="toastNotification.removeNotification(toast)"
        :key="toast"
      >
        <p>{{ toast }}</p>
      </li>
    </TransitionGroup>
  </div>
</template>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease, transform 0.2s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: scale(90%);
}

.toast-list {
  position: relative;
  align-items: center;
  gap: 0.5rem;
}

.toast-notification {
  background-color: rgba(0, 0, 0, 0.5);
  box-shadow: var(--shadow-floating);
  color: white;
  text-align: center;
  width: fit-content;
  min-width: 5rem;
  padding: 1rem;
  border-radius: 6px;
  pointer-events: all;
}

.toast-viewport {
  position: absolute;
  overflow: hidden;
  pointer-events: none;
  /* border: 1px solid red; */
  top: 65%;
  left: 5%;
  right: 5%;
  bottom: 2%;
}
</style>
