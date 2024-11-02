<script setup lang="ts">
import { ref } from 'vue'
import Select from './components/Select/Select.vue'
import Option from './components/Select/Option.vue'

const singleValue = ref<string>('')
const multipleValue = ref<string[]>([])

const handleSingleChange = (value: string | number) => {
  console.log('Single Selected:', value)
}

const handleMultipleChange = (value: (string | number)[]) => {
  console.log('Multiple Selected:', value)
}
</script>

<template>
  <div class="container">
    <h1>Select Component Demo</h1>
    
    <div class="select-demo">
      <h2>Single Select</h2>
      <Select
        v-model="singleValue"
        placeholder="Choose an option"
        @change="handleSingleChange"
      >
        <Option value="1" label="Option 1" />
        <Option value="2" label="Option 2" />
        <Option value="3" label="Option 3" />
        <Option value="4" label="Option 4" disabled />
      </Select>
      
      <div class="selected-value">
        Selected value: {{ singleValue }}
      </div>

      <h2>Multiple Select</h2>
      <Select
        v-model="multipleValue"
        placeholder="Choose multiple options"
        multiple
        @change="handleMultipleChange"
      >
        <Option value="1" label="Option 1" />
        <Option value="2" label="Option 2" />
        <Option value="3" label="Option 3" />
        <Option value="4" label="Option 4" disabled />
        <Option value="5" label="Option 5" />
        <Option value="6" label="Option 6" />
      </Select>
      
      <div class="selected-value">
        Selected values: {{ multipleValue }}
      </div>
    </div>
  </div>
</template>

<style>
.container {
  padding: 20px;
}

.select-demo {
  max-width: 300px;
  margin: 20px auto;
}

.selected-value {
  margin-top: 20px;
  color: #409eff;
}

h2 {
  margin: 20px 0 10px;
  font-size: 1.2em;
  color: #606266;
}
</style>