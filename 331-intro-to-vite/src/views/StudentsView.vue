<script setup lang="ts">
import { ref, onMounted } from 'vue'
import StudentService from '@/services/StudentService'

type Student = {
  id?: number
  name: string
  surname: string
  gpa: number
}

const students = ref<Student[] | null>(null)

onMounted(() => {
  StudentService.getStudents().then((response) => {
    students.value = response.data
  })
})
</script>

<template>
  <h1>Students</h1>
  <div class="students">
    <div
      v-for="(student, index) in students || []"
      :key="student.id ?? index"
      class="card"
    >
      <h2>{{ student.name }} {{ student.surname }}</h2>
      <p>GPA: {{ student.gpa }}</p>
    </div>
  </div>
</template>

<style scoped>
.students {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.card {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 12px;
  margin: 8px 0;
  width: 320px;
  box-shadow: rgba(0, 0, 0, 0.05) 0 2px 6px;
}
</style>