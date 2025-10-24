<script setup lang="ts">
import { toRefs, ref } from 'vue'
import { type Event } from '@/types'
import { useRouter } from 'vue-router'

const props = defineProps<{
  event: Event
}>()
const { event } = toRefs(props)

const router = useRouter()
const status = ref<string | null>(null)

const register = () => {
  // Simulate register success
  status.value = 'Registered successfully! Redirecting to details...'
  // Programmatic navigation to event detail view
  router.push({ name: 'event-detail-view', params: { id: event.value.id } })
}
</script>

<template>
  <p>Register event here</p>
  <button @click="register">Register</button>
  <p v-if="status">{{ status }}</p>
</template>