<script setup lang="ts">
import { ref } from 'vue'
import ModalWindow from './components/ModalWindow.vue'

const isModalOpen = ref(false)
const selectedFolderId = ref<number | null>(null)

const handleOpenModal = () => {
  isModalOpen.value = true
}

const handleCloseModal = () => {
  isModalOpen.value = false
}

const handleSelect = (folderId: number) => {
  selectedFolderId.value = folderId
  console.log(`Выбрана папка с ID: ${folderId}`)
}
</script>

<template>
  <main>
    <button @click="handleOpenModal">Открыть</button>
    <ModalWindow
      v-if="isModalOpen"
      title="Выберите папку"
      @close="handleCloseModal"
      @select="handleSelect"
    />
  </main>
</template>
