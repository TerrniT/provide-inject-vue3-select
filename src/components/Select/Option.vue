<script setup lang="ts">
import { computed, inject, onMounted } from 'vue'

const props = defineProps<{
  value: string | number
  label?: string
  disabled?: boolean
}>()

type SelectContext = {
  selectedValue: { value: (string | number)[] | string | number | undefined }
  multiple?: boolean
  onSelect: (value: string | number, label: string) => void
}

const selectContext = inject<SelectContext>('select-context')
const registerOption = inject<(value: string | number, label: string) => void>('register-option')

const isSelected = computed(() => {
  if (!selectContext?.selectedValue.value) return false
  
  if (Array.isArray(selectContext.selectedValue.value)) {
    return selectContext.selectedValue.value.includes(props.value)
  }
  return selectContext.selectedValue.value === props.value
})

const handleClick = () => {
  if (!props.disabled && selectContext) {
    selectContext.onSelect(props.value, props.label || props.value.toString())
  }
}

onMounted(() => {
  if (registerOption) {
    registerOption(props.value, props.label || props.value.toString())
  }
})
</script>

<template>
  <div 
    class="select-option"
    :class="{ 
      'is-selected': isSelected,
      'is-disabled': disabled 
    }"
    @click="handleClick"
  >
    {{ label || value }}
  </div>
</template>

<style scoped>
.select-option {
  padding: 8px 12px;
  cursor: pointer;
  color: #606266;
}

.select-option:hover:not(.is-disabled) {
  background-color: #f5f7fa;
}

.select-option.is-selected {
  color: #409eff;
  font-weight: bold;
  background-color: #f5f7fa;
}

.select-option.is-disabled {
  color: #c0c4cc;
  cursor: not-allowed;
}
</style>