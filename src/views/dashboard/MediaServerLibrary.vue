<script setup lang="ts">
import api from '@/api'
import type { MediaServerPlayItem } from '@/api/types'
import LibraryCard from '@/components/cards/LibraryCard.vue'

// 媒体库列表
const libraryList = ref<MediaServerPlayItem[]>([])

// 调用API查询
async function loadLibrary() {
  try {
    libraryList.value = await api.get('mediaserver/library')
  } catch (e) {
    console.log(e)
  }
}

onMounted(() => {
  loadLibrary()
})
</script>

<template>
  <VCard>
    <VCardItem>
      <template #append>
        <VIcon class="cursor-move">mdi-drag</VIcon>
      </template>
      <VCardTitle >我的媒体库</VCardTitle>
    </VCardItem>

    <div v-if="libraryList.length > 0" class="grid gap-4 grid-backdrop-card mx-3" tabindex="0">
      <LibraryCard v-for="data in libraryList" :key="data.id" :media="data" height="10rem" />
    </div>
  </VCard>
</template>

<style lang="scss">
.grid-backdrop-card {
  grid-template-columns: repeat(auto-fill, minmax(15rem, 1fr));
  padding-block-end: 1rem;
}
</style>
