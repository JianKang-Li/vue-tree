<template>
  <ul ref="el" class="k-tree">
    <template v-for="(node, index) in nodes" :key="index">
      <TreeItem :node="node" :options="options" :level="level" />
    </template>
  </ul>
</template>

<script setup>
import { nextTick, ref, h, defineComponent, computed } from 'vue'
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
  },
  level: {
    type: Number,
    default: 0
  }
})

const el = ref(null)
const nodes = ref(props.nodes)
const level = computed(() => {
  return props.level + 1
})

</script>

<style scoped>
ul.k-tree {
  list-style: none;
  padding: 0;
}

ul.k-tree ul {
  list-style: none;
  padding: 0 0 0 18px;
}
</style>
