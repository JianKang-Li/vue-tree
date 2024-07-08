<template>
  <li>
    <input type="checkbox" v-show="options.showCheckBox">
    {{ node.name }}
  </li>
</template>

<script setup>
import { ref } from 'vue'
import defineOptions from './Options'

const props = defineProps({
  node: {
    type: Object,
    default: () => ({
      id: 0,
      name: 'tree-node',
    })
  },
  options: {
    type: Object,
    default: () => (defaultOptions)
  }
})

const options = ref(props.options)
const node = ref(props.node)
</script>
