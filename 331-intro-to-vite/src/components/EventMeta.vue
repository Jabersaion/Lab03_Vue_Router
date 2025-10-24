<script setup lang="ts">
import type { Event } from '@/types'

defineProps<{
  event: Event
}>()
</script>

<template>
  <div class="event-meta">
    <span class="meta-category">{{ event.category }}</span>
    <span class="meta-organizer">{{ event.organizer }}</span>
  </div>
</template>

<style scoped>
.event-meta {
  text-align: right;
}
.event-meta .meta-category,
.event-meta .meta-organizer {
  font-size: 16px;
  display: block;
}
</style>