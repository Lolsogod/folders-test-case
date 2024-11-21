<script setup lang="ts">
import { ref } from 'vue'
import type { FolderType } from '../mock/folders'

defineProps<{
  folders: FolderType[]
  selectedId: number | null
}>()

const emit = defineEmits<{
  select: [id: number]
}>()

const expandedFolders = ref<Set<number>>(new Set())

const toggleFolder = (id: number) => {
  if (expandedFolders.value.has(id)) {
    expandedFolders.value.delete(id)
  } else {
    expandedFolders.value.add(id)
  }
}
</script>

<template>
  <div class="folder-tree">
    <div v-for="folder in folders" :key="folder.id" class="folder-item">
      <div
        class="folder"
        :class="{ selected: selectedId === folder.id }"
        @click="emit('select', folder.id)"
      >
        <span
          v-if="folder.children.length"
          class="toggle"
          @click.stop="toggleFolder(folder.id)"
        >
          {{ expandedFolders.has(folder.id) ? '▼' : '▶' }}
        </span>
        {{ folder.name }}
      </div>
      <div
        v-if="folder.children.length && expandedFolders.has(folder.id)"
        class="children"
      >
        <FolderTree
          :folders="folder.children"
          :selected-id="selectedId"
          @select="emit('select', $event)"
        />
      </div>
    </div>
  </div>
</template>

<style scoped>
.folder-tree {
  margin-left: 20px;
}

.folder {
  padding: 4px;
  cursor: pointer;
  user-select: none;
}

.folder:hover {
  background: #f0f0f0;
}

.folder.selected {
  background: #e0e0e0;
}

.toggle {
  display: inline-block;
  width: 20px;
  cursor: pointer;
}

.children {
  margin-left: 20px;
}
</style>
