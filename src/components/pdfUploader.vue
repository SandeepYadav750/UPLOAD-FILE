<template>
    <div class="min-h-screen bg-gray-900 flex items-center justify-center px-4">
        <div class="bg-white w-full max-w-2xl p-8 rounded-xl  text-black shadow-lg">
            <div class="flex items-center justify-between mb-4">
                <span>
                    <h2 class="text-2xl font-bold">Upload</h2>
                    <p class="text-gray-500">Add your Documents here, and you can upload up to 5 files max.</p>
                </span>
                <span><img src="../assets/cross.svg" alt="cancel" class="w-4 cursor-pointer " /></span>
            </div>

            <!-- Drag and Drop Area -->
            <div class="border-2 border-dashed border-gray-300 rounded-lg p-6 hover:border-blue-400 transition">
                <div class="text-center cursor-pointer " @dragover.prevent @drop.prevent="handleDrop"
                    @click="fileInput.click()">
                    <span class="flex items-center justify-center"><img src="../assets/upArrow.svg" alt="uploader"
                            class="w-24" /></span>
                    <p class="text-gray-500 text-sm font-semibold">Drag your file(s) to start uploading</p>
                    <input type="file" ref="fileInput" multiple accept="application/pdf" class="hidden"
                        @change="handleFileChange" />
                    <div class="px-4 flex items-center justify-center gap-2 m-4"><span
                            class="w-32 h-[1px] bg-gray-200"></span>OR<span class="w-32 h-[1px] bg-gray-200"></span>
                    </div>
                </div>
                <!-- Buttons -->
                <div class="flex flex-col sm:flex-row gap-4 ">
                    <button @click.prevent="fileInput.click()"
                        class="w-full sm:w-1/2 px-4 py-2 border-2 border-blue-600 text-blue-600 font-semibold cursor-pointer rounded-lg hover:bg-blue-700 hover:text-white transition">
                        Browse Files
                    </button>
                    <input v-model="websiteLink" type="url" placeholder="Enter website link"
                        class="text-center w-full sm:w-1/2 px-4 py-2 border-2 border-blue-600 text-blue-600 font-semibold rounded-lg  focus:outline-none focus:ring focus:ring-blue-300" />
                </div>
            </div>
            <div v-if="errorPDF" class="text-red-500 mt-2 text-sm">{{ errorPDF }}</div>


            <!-- File List -->
            <ul class="mt-3 text-sm text-gray-700">
                <li v-for="(file, index) in files" :key="index" class="mt-1">
                    {{ file.name }}
                </li>
            </ul>

            <!-- Email Field -->
            <div class="mt-4">
                <!-- <label class="block text-sm font-medium mb-1">Enter your email</label> -->
                <input v-model="email" type="email" placeholder="Enter your email"
                    class="w-full px-4 py-2 font-semibold border text-center rounded-lg focus:outline-none focus:ring focus:ring-blue-300" />
            </div>

            <!-- Error Message -->
            <div v-if="error" class="text-red-500 mt-2 text-sm">{{ error }}</div>

            <!-- Submit Button -->
            <button
                class="w-full mt-6 py-3 font-semibold border hover:bg-green-600 hover:text-white cursor-pointer rounded-lg text-lg transition"
                @click="submitForm">
                Generate Schedule
            </button>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue'

const files = ref([])
const fileInput = ref(null)
const email = ref('')
const websiteLink = ref('')
const errorPDF = ref('')
const error = ref('')

function handleDrop(e) {
    const droppedFiles = [...e.dataTransfer.files]
    processFiles(droppedFiles)
}

function handleFileChange(e) {
    const selectedFiles = [...e.target.files]
    processFiles(selectedFiles)
}

// function processFiles(newFiles) {
//     const allFiles = [...files.value, ...newFiles].slice(0, 5)

//     const invalid = allFiles.find(file => file.type !== 'application/pdf')
//     if (invalid) {
//         error.value = 'Only PDF files are allowed.'
//         return
//     }

//     files.value = allFiles
//     error.value = ''
// }

function isValidEmail(email) {
    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)
}

function processFiles(newFiles) {
    const combinedFiles = [...files.value, ...newFiles]

    if (combinedFiles.length > 5) {
        errorPDF.value = 'Maximum 5 files accepted.'
        return
    }

    const invalid = newFiles.find(file => file.type !== 'application/pdf')
    if (invalid) {
        errorPDF.value = 'Only PDF files are allowed.'
        return
    }

    files.value = combinedFiles
    error.value = ''
    errorPDF.value = ''
}

function submitForm() {
    if (files.value.length === 0) {
        errorPDF.value = 'Please upload at least one PDF file.'
        return
    }

    if (!isValidEmail(email.value)) {
        error.value = 'Please enter a valid email address.'
        return
    }

    // Clear everything on success
    alert('Form submitted successfully!')
    files.value = []
    email.value = ''
    websiteLink.value = ''
    fileInput.value.value = null
    error.value = ''
    errorPDF.value = ''
}
</script>

<style scoped>
input:focus {
    outline: none;
    border-color: #3b82f6;
}
</style>