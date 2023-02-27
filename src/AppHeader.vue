<script setup lang="ts">
import { onMounted, ref, watch } from "vue";
import ContextMenuItem from "../Models/ContextMenuItem";
import { useContextMenuStore } from "../Stores/ContextMenuStore";
import { useServerStore } from "../Stores/ServerStore";

const serverStore = useServerStore();
const contextMenuStore = useContextMenuStore();
</script>

<template>
  <header>
    <ol>
      <li v-for="contextMenuItem in contextMenuStore.contextMenuItems">
        <a @click="contextMenuItem?.action" class="button borderless background-static">{{ contextMenuItem.name }}</a>
      </li>
    </ol>

    <div>
      <RouterLink
        to="/servers"
        class="button borderless background-static"
        :class="!serverStore.connected ? 'disabled' : ''"
        >{{ serverStore.connected ? serverStore.nickname : "Disconnected" }}</RouterLink
      >
    </div>
  </header>
</template>

<style scoped>
.visibility-transition {
  transition: opacity 0.4s var(--animation-presentation);
}

header {
  transition: transform 0.4s var(--animation-presentation);
  background-color: var(--background-panel);
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 2rem;
  padding: 0px 0.5rem;
  border-bottom: 1px solid var(--border-slight);
  z-index: 5;
}

header ol {
  display: flex;
  list-style: none;
  gap: 0.15rem;
  align-items: center;
  margin: 0px;
  padding: 0px;
}
</style>
