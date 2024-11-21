<script setup lang="ts">
import { ref } from 'vue'
import FolderTree from './FolderTree.vue'
import { mockFolders } from '../mock/folders'

defineProps<{
  title: string
}>()

const emit = defineEmits<{
  close: []
  select: [folderId: number]
}>()

const selectedFolderId = ref<number | null>(null)

const handleSelect = (id: number) => {
  selectedFolderId.value = id
}

const handleOk = () => {
  if (selectedFolderId.value) {
    emit('select', selectedFolderId.value)
    emit('close')
  }
}
</script>

<template>
  <div class="modal-overlay">
    <div class="modal">
      <h2>{{ title }}</h2>
      <FolderTree :folders="mockFolders" :selected-id="selectedFolderId" @select="handleSelect" />
      <div class="modal-buttons">
        <button @click="handleOk" :disabled="!selectedFolderId">Ок</button>
        <button @click="emit('close')">Закрыть</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  background: white;
  padding: 20px;
  min-width: 300px;
}

.modal-buttons {
  display: flex;
  gap: 10px;
  justify-content: flex-end;
}

</style>
