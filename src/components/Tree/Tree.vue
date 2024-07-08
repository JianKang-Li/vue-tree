<template>
  <ul ref="el">
    <template v-for="(node, index) in nodes" :key="index">
      <TreeItem :node="node" :options="options"/>
    </template>
  </ul>
</template>

<script setup>
import { nextTick, ref, h, defineComponent } from 'vue'
import TreeItem from './TreeItem.vue'
import defaultOptions from "./Options"

const props = defineProps({
  nodes: {
    type: Array,
    default: () => []
  },
  options: {
    type: Object,
    default: () => (defaultOptions)
  }
})

const el = ref(null)
const nodes = ref(props.nodes)

</script>
