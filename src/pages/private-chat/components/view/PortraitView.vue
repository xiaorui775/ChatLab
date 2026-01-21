<script setup lang="ts">
import { useI18n } from 'vue-i18n'

const { t } = useI18n()

interface TimeFilter {
  startTs?: number
  endTs?: number
}

// Props
defineProps<{
  sessionId: string
  timeFilter?: TimeFilter
}>()
</script>

<template>
  <div class="main-content flex h-full items-center justify-center p-6">
    <div
      class="flex h-full w-full items-center justify-center rounded-xl border-2 border-dashed border-gray-300 bg-gray-50 dark:border-gray-700 dark:bg-gray-900/50"
    >
      <div class="text-center">
        <UIcon name="i-heroicons-user-circle" class="mx-auto h-12 w-12 text-gray-400" />
        <p class="mt-3 text-sm font-medium text-gray-600 dark:text-gray-400">
          {{ t('title') }}
        </p>
        <p class="mt-1 max-w-md px-4 text-sm text-gray-500">
          {{ t('description') }}
        </p>
      </div>
    </div>
  </div>
</template>

<i18n>
{
  "zh-CN": {
    "title": "对话画像",
    "description": "双方发言特征对比（消息长度、表情使用、消息类型等）"
  },
  "en-US": {
    "title": "Chat Portrait",
    "description": "Comparison of chat characteristics (message length, emoji usage, message types, etc.)"
  }
}
</i18n>

