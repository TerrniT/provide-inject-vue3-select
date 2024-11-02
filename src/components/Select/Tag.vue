<script setup lang="ts">
defineProps<{
  closable?: boolean
}>()

defineEmits<{
  close: []
}>()
</script>

<template>
  <span class="tag">
    <slot></slot>
    <span 
      v-if="closable" 
      class="tag-close" 
      @click.stop="$emit('close')"
      role="button"
      aria-label="Remove tag"
    >
      ×
    </span>
  </span>
</template>

<style scoped>
.tag {
  background-color: #ecf5ff;
  border: 1px solid #d9ecff;
  border-radius: 4px;
  padding: 0 8px;
  height: 24px;
  line-height: 22px;
  font-size: 12px;
  color: #409eff;
  display: inline-flex;
  align-items: center;
  gap: 4px;
  transition: all 0.2s;
}

.tag:hover {
  background-color: #d9ecff;
}

.tag-close {
  cursor: pointer;
  font-size: 14px;
  font-weight: bold;
  color: #409eff;
  transition: color 0.2s;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 16px;
  height: 16px;
  border-radius: 50%;
}

.tag-close:hover {
  background-color: #409eff;
  color: #fff;
}
</style>