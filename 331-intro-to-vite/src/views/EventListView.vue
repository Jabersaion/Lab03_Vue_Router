<script setup lang="ts">
import { ref, onMounted } from 'vue'
import type Event from '@/types/Event'
import EventService from '@/services/EventService'
import EventCard from '@/components/EventCard.vue'

const events = ref<Event[]>([])
const loading = ref(true)
const error = ref<string | null>(null)

onMounted(() => {
  EventService.getEvents()
    .then((response) => {
      events.value = response.data
      loading.value = false
    })
    .catch((err) => {
      error.value = '加载事件失败，请检查 Mock 服务是否在 3000 端口运行。'
      console.error('There was an error!', err)
      loading.value = false
    })
})
</script>

<template>
  <div class="event-list">
    <h1>Events</h1>
    <div v-if="loading">正在加载事件...</div>
    <div v-else-if="error">{{ error }}</div>
    <div v-else>
      <EventCard v-for="evt in events" :key="evt.id" :event="evt" />
    </div>
  </div>
</template>