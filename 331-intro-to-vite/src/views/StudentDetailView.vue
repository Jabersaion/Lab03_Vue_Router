<script setup lang="ts">
import { ref, onMounted } from 'vue'
import StudentService from '@/services/StudentService'

type Student = {
  id?: number
  studentId?: string
  name: string
  surname: string
  gpa: number
  image?: string
  penAmount?: number
  description?: string
}

const student = ref<Student | null>(null)
const imageSrc = ref<string | null>(null)

function sanitizeImage(url?: string) {
  if (!url) return null
  // 去除反引号和两侧空格
  return url.replace(/`/g, '').trim()
}

function handleImgError() {
  imageSrc.value = 'https://via.placeholder.com/300x200?text=No+Image'
}

onMounted(() => {
  StudentService.getStudent().then((response) => {
    student.value = response.data
    imageSrc.value = sanitizeImage(response.data?.image)
  })
})
</script>

<template>
  <h1>Student Detail</h1>
  <div v-if="student" class="card">
    <img
      v-if="imageSrc"
      :src="imageSrc"
      alt="Student image"
      @error="handleImgError"
      class="image"
    />
    <h2>{{ student.name }} {{ student.surname }}</h2>
    <p>ID: {{ student.studentId ?? student.id }}</p>
    <p>GPA: {{ student.gpa }}</p>
    <p v-if="student.penAmount !== undefined">Pens: {{ student.penAmount }}</p>
    <p v-if="student.description">{{ student.description }}</p>
  </div>
</template>

<style scoped>
.card {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 12px;
  margin: 12px auto;
  width: 360px;
  box-shadow: rgba(0, 0, 0, 0.05) 0 2px 6px;
}
.image {
  width: 320px;
  height: auto;
  object-fit: cover;
  border-radius: 8px;
  margin-bottom: 12px;
}
</style>