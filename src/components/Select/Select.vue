<script setup lang="ts">
import { computed, provide, onMounted, onUnmounted } from 'vue'
import { useSelect } from '../../composables/useSelect'
import Tag from './Tag.vue'

const props = withDefaults(defineProps<{
  modelValue: (string | number)[] | string | number | undefined
  placeholder?: string
  disabled?: boolean
  multiple?: boolean
}>(), {
  placeholder: 'Select',
  disabled: false,
  multiple: false
})

const emit = defineEmits<{
  'update:modelValue': [value: (string | number)[] | string | number]
  'change': [value: (string | number)[] | string | number]
}>()

const {
  isOpen,
  selectedOptions,
  selectOption,
  registerOption,
  toggleDropdown,
  removeTag,
  closeDropdown
} = useSelect(props, emit)

const displayValue = computed(() => {
  if (selectedOptions.value.length === 0) return props.placeholder
  if (!props.multiple) return selectedOptions.value[0]?.label
  return ''
})

// Provide context for child components
provide('select-context', {
  selectedValue: computed(() => props.modelValue),
  multiple: props.multiple,
  onSelect: selectOption
})

provide('register-option', registerOption)

onMounted(() => {
  document.addEventListener('click', closeDropdown)
})

onUnmounted(() => {
  document.removeEventListener('click', closeDropdown)
})
</script>

<template>
  <div class="select-container" :class="{ disabled }">
    <div 
      class="select-trigger"
      @click="toggleDropdown"
      :class="{ 
        'is-open': isOpen,
        'has-tags': multiple && selectedOptions.length > 0 
      }"
    >
      <div class="select-values">
        <template v-if="multiple && selectedOptions.length > 0">
          <Tag 
            v-for="option in selectedOptions" 
            :key="option.value"
            :closable="!disabled"
            @close="removeTag(option.value)"
          >
            {{ option.label }}
          </Tag>
        </template>
        <span v-else class="select-label" :class="{ 'is-placeholder': !selectedOptions.length }">
          {{ displayValue }}
        </span>
      </div>
      <span class="select-arrow" :class="{ 'is-open': isOpen }">▼</span>
    </div>
    
    <div v-show="isOpen" class="select-dropdown">
      <slot></slot>
    </div>
  </div>
</template>

<style scoped>
.select-container {
  position: relative;
  width: 100%;
  min-width: 200px;
  user-select: none;
}

.select-trigger {
  min-height: 40px;
  padding: 4px 30px 4px 12px;
  border: 1px solid #dcdfe6;
  border-radius: 4px;
  background-color: #fff;
  cursor: pointer;
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: all 0.2s;
}

.select-trigger:hover:not(.disabled) {
  border-color: #409eff;
}

.select-trigger.is-open {
  border-color: #409eff;
}

.select-values {
  display: flex;
  flex-wrap: wrap;
  gap: 4px;
  flex: 1;
  min-height: 30px;
  align-items: center;
  padding: 2px 0;
}

.select-label {
  color: #606266;
}

.select-label.is-placeholder {
  color: #a8abb2;
}

.select-arrow {
  font-size: 12px;
  color: #c0c4cc;
  transition: transform 0.2s;
  position: absolute;
  right: 8px;
}

.select-arrow.is-open {
  transform: rotate(180deg);
}

.select-dropdown {
  position: absolute;
  top: calc(100% + 5px);
  left: 0;
  width: 100%;
  background-color: #fff;
  border: 1px solid #e4e7ed;
  border-radius: 4px;
  box-shadow: 0 2px 12px 0 rgba(0,0,0,.1);
  z-index: 1000;
  max-height: 200px;
  overflow-y: auto;
}

.disabled {
  opacity: 0.7;
  cursor: not-allowed;
}

.disabled .select-trigger {
  cursor: not-allowed;
  background-color: #f5f7fa;
}

.has-tags {
  height: auto;
  min-height: 40px;
}
</style>