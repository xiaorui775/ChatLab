<script setup lang="ts">
import { computed, watch, onMounted } from 'vue'
import { useRoute, useRouter } from 'vue-router'

/**
 * 轻量级子标签页组件
 * 适用于页面内部的二级导航,使用原生样式
 * 支持通过 persistKey 将选中状态同步到 URL 查询参数
 */
interface TabItem {
  id: string
  label: string
  icon?: string
}

interface Props {
  modelValue: string
  items: TabItem[]
  /** 持久化 key，设置后会将当前 tab 状态同步到 URL 查询参数 */
  persistKey?: string
}

interface Emits {
  (e: 'update:modelValue', value: string): void
  (e: 'change', value: string): void
}

const props = defineProps<Props>()
const emit = defineEmits<Emits>()

const route = useRoute()
const router = useRouter()

// 计算内部值
const activeTab = computed({
  get: () => props.modelValue,
  set: (value) => {
    emit('update:modelValue', value)
    emit('change', value)
  },
})

// 点击标签
const handleTabClick = (tabId: string) => {
  activeTab.value = tabId
}

// 从 URL 查询参数恢复 tab 状态
onMounted(() => {
  if (props.persistKey) {
    const savedTab = route.query[props.persistKey] as string
    // 验证 savedTab 是否在 items 中存在
    if (savedTab && props.items.some((item) => item.id === savedTab)) {
      activeTab.value = savedTab
    }
  }
})

// 监听 tab 变化，同步到 URL 查询参数
watch(
  () => props.modelValue,
  (newValue) => {
    if (props.persistKey && newValue) {
      // 使用 replace 而不是 push，避免产生大量历史记录
      router.replace({
        query: {
          ...route.query,
          [props.persistKey]: newValue,
        },
      })
    }
  }
)
</script>

<template>
  <div class="border-b border-gray-200 bg-white px-6 dark:border-gray-800 dark:bg-gray-900">
    <div class="flex gap-1">
      <button
        v-for="tab in items"
        :key="tab.id"
        class="flex items-center gap-2 border-b-2 px-4 py-3 text-sm font-medium transition-colors"
        :class="[
          activeTab === tab.id
            ? 'border-primary-500 text-primary-600 dark:text-primary-400'
            : 'border-transparent text-gray-500 hover:border-gray-300 hover:text-gray-700 dark:text-gray-400 dark:hover:text-gray-300',
        ]"
        @click="handleTabClick(tab.id)"
      >
        <UIcon v-if="tab.icon" :name="tab.icon" class="h-4 w-4" />
        {{ tab.label }}
      </button>
    </div>
  </div>
</template>
