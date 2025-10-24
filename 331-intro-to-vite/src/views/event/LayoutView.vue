<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { type Event } from '@/types'
import EventService from '@/services/EventService'
import { useRouter } from 'vue-router'

const { id } = defineProps({
  id: {
    type: String,
    required: true
  }
})

const event = ref<Event | null>(null)
const router = useRouter()

onMounted(() => {
  EventService.getEvent(parseInt(id, 10))
    .then((response) => {
      event.value = response.data
    })
    .catch((error) => {
      const status = (error as any)?.response?.status
      if (status === 404) {
        router.push({
          name: '404-resource-view',
          params: { resource: 'event' }
        })
      } else {
        router.push({ name: 'network-error-view' })
      }
    })
})
</script>

<template>
  <div v-if="event" class="event-layout">
    <h1>{{ event.title }}</h1>
    <nav class="event-nav">
      <RouterLink :to="{ name: 'event-detail-view', params: { id } }">Details</RouterLink>
      |
      <RouterLink :to="{ name: 'event-register-view', params: { id } }">Register</RouterLink>
      |
      <RouterLink :to="{ name: 'event-edit-view', params: { id } }">Edit</RouterLink>
    </nav>
    <RouterView :event="event" />
  </div>
</template>

<style scoped>
.event-layout {
  text-align: center;
}
.event-nav {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 12px;
  margin: 8px 0 16px;
}
.event-nav a {
  text-decoration: none;
  color: #2c3e50;
}
</style>