<template>
  <div class="max-w-xl mx-auto p-6 bg-white rounded-lg shadow-md mt-10">
    <h2 class="text-2xl font-semibold mb-4">Upload PDF Files</h2>

    <!-- Drag and Drop Zone -->
    <div class="border-2 border-dashed border-gray-300 rounded-md p-6 text-center cursor-pointer" @dragover.prevent
      @drop.prevent="handleDrop" @click="fileInput.click()">
      <p class="text-gray-500">Drag & Drop PDF files here</p>
      <p class="text-gray-500 mt-1">or</p>
      <button class="mt-2 px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600"
        @click.prevent="fileInput.click()">Browse Files</button>
      <input type="file" ref="fileInput" multiple accept="application/pdf" class="hidden" @change="handleFileChange" />
    </div>

    <ul class="mt-4 text-sm text-gray-700">
      <li v-for="(file, index) in files" :key="index" class="mt-1">
        {{ file.name }}
      </li>
    </ul>

    <div v-if="error" class="text-red-500 mt-2">{{ error }}</div>

    <!-- Website Link Input -->
    <div class="mt-4">
      <label class="block mb-1">Enter Website Link</label>
      <input v-model="websiteLink" type="url" class="w-full border px-3 py-2 rounded"
        placeholder="https://example.com" />
    </div>

    <!-- Email Input -->
    <div class="mt-4">
      <label class="block mb-1">Enter your email</label>
      <input v-model="email" type="email" class="w-full border px-3 py-2 rounded" placeholder="you@example.com" />
    </div>

    <!-- Generate Button -->
    <button class="mt-6 w-full bg-green-600 text-white py-2 rounded hover:bg-green-700" @click="submitForm">
      Generate Schedule
    </button>
  </div>
</template>

<script>
export default {
  name: 'PDF_Upload',
}
import { ref } from 'vue'

const files = ref([])
const fileInput = ref(null)
const email = ref('')
const websiteLink = ref('')
const error = ref('')

function handleDrop(e) {
  const droppedFiles = [...e.dataTransfer.files]
  processFiles(droppedFiles)
}

function handleFileChange(e) {
  const selectedFiles = [...e.target.files]
  processFiles(selectedFiles)
}

function processFiles(newFiles) {
  const allFiles = [...files.value, ...newFiles].slice(0, 5)

  const invalid = allFiles.find(file => file.type !== 'application/pdf')
  if (invalid) {
    error.value = 'Only PDF files are allowed.'
    return
  }

  files.value = allFiles
  error.value = ''
}

function isValidEmail(email) {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)
}

function submitForm() {
  if (files.value.length === 0) {
    error.value = 'Please upload at least one PDF file.'
    return
  }

  if (!isValidEmail(email.value)) {
    error.value = 'Please enter a valid email address.'
    return
  }

  error.value = ''
  // Simulate submission
  alert('Submitted successfully!')
}
</script>

<style scoped>
input:focus {
  outline: none;
  border-color: #3b82f6;
}
</style>
