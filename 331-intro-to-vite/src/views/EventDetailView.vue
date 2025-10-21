<script setup lang="ts">
import { ref, onMounted, defineProps } from 'vue'
import type Event from '@/types/Event'
import EventService from '@/services/EventService'

const props = defineProps<{ id: string }>()

const event = ref<Event | null>(null)
const loading = ref(true)
const error = ref<string | null>(null)

onMounted(() => {
  EventService.getEvent(Number(props.id))
    .then((response) => {
      event.value = response.data
      loading.value = false
    })
    .catch((err) => {
      error.value = '加载事件失败或未找到该事件。'
      console.error('There was an error!', err)
      loading.value = false
    })
})
</script>

<template>
  <div>
    <div v-if="loading">正在加载详情...</div>
    <div v-else-if="error">{{ error }}</div>
    <div v-else>
      <div v-if="event">
        <h1>{{ event.title }}</h1>
        <p>{{ event.time }} on {{ event.date }} @ {{ event.location }}</p>
        <p>{{ event.description }}</p>
      </div>
      <div v-else>未找到该事件。</div>
    </div>
  </div>
</template>