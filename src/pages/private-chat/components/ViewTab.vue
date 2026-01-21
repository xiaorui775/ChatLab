<script setup lang="ts">
import { ref, computed } from 'vue'
import { useI18n } from 'vue-i18n'
import { SubTabs } from '@/components/UI'
import MessageView from './view/MessageView.vue'
import WordcloudView from './view/WordcloudView.vue'
import PortraitView from './view/PortraitView.vue'

const { t } = useI18n()

interface TimeFilter {
  startTs?: number
  endTs?: number
}

// Props
const props = defineProps<{
  sessionId: string
  timeFilter?: TimeFilter
}>()

// 子 Tab 配置
const subTabs = computed(() => [
  { id: 'message', label: t('message'), icon: 'i-heroicons-chat-bubble-left-right' },
  { id: 'wordcloud', label: t('wordcloud'), icon: 'i-heroicons-cloud' },
  { id: 'portrait', label: t('portrait'), icon: 'i-heroicons-user-circle' },
])

const activeSubTab = ref('message')
</script>

<template>
  <div class="flex h-full flex-col">
    <!-- 子 Tab 导航 -->
    <SubTabs v-model="activeSubTab" :items="subTabs" persist-key="privateViewTab" />

    <!-- 子 Tab 内容 -->
    <div class="flex-1 min-h-0 overflow-auto">
      <Transition name="fade" mode="out-in">
        <!-- 消息 -->
        <MessageView
          v-if="activeSubTab === 'message'"
          :session-id="props.sessionId"
          :time-filter="props.timeFilter"
        />

        <!-- 词云 -->
        <WordcloudView
          v-else-if="activeSubTab === 'wordcloud'"
          :session-id="props.sessionId"
          :time-filter="props.timeFilter"
        />

        <!-- 对话画像 -->
        <PortraitView
          v-else-if="activeSubTab === 'portrait'"
          :session-id="props.sessionId"
          :time-filter="props.timeFilter"
        />
      </Transition>
    </div>
  </div>
</template>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.15s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>

<i18n>
{
  "zh-CN": {
    "message": "消息",
    "wordcloud": "词云",
    "portrait": "对话画像"
  },
  "en-US": {
    "message": "Messages",
    "wordcloud": "Word Cloud",
    "portrait": "Chat Portrait"
  }
}
</i18n>

