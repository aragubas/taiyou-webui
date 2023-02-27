<script setup lang="ts">
import Contact from "../Models/Contact";

const props = defineProps<{
  contact: Contact;
}>();
</script>

<template>
  <div class="button-like-nonclickable contact-card">
    <span class="placeholder"></span>
    <div>
      <p>{{ contact.fullName }}</p>
      <code>{{ contact.phoneNumber ? `${contact.phoneNumber} - ${contact.emailAddress}` : contact.emailAddress }}</code>
    </div>
  </div>
</template>

<style scoped>
.contact-card {
  display: flex;
  gap: 0.5rem;
  padding: 0.25rem;
  border-radius: 4px;
}

.contact-card p {
  font-size: 1.25rem;
}
</style>
