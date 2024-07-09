<template>
  <ul ref="el" class="k-tree">
    <template v-for="(node, index) in nodes" :key="index">
      <TreeItem :node="node" :options="options" :level="level" @change-check="changeItem"/>
    </template>
  </ul>
</template>

<script setup>
import { nextTick, ref, h, defineComponent, computed } from 'vue'
import TreeItem from './TreeItem.vue'
import { defaultOptions } from "./utils/util"

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

const changeNode = (arr, id, node) => {
  for (let i = 0; i < arr.length; i++) {
    if (arr[i].id === id) {
      for(const key in node) {
        arr[i][key] = node[key]
      }
    }

    if (arr[i].children && arr[i].children.length) {
      changeNode(arr[i].children, id, node)
    }
  }
  return
}

const getNodes = (arr, key, value) => {
  const nodes = []

  for (let i = 0; i < arr.length; i++) {
    if (arr[i][key] === value) {
      nodes.push(arr[i])
    }

    if (arr[i].children && arr[i].children.length) {
      const subNodes = getNodes(arr[i].children, key, value)

      nodes.push(...subNodes)
    }
  }
  return nodes
}

const changeItem = (status, id) => {
  changeNode(nodes.value, id, {checked: status})
}

const getCheckedNodes = () => {
  return getNodes(nodes.value, 'checked' , true)
}

defineExpose({
  getCheckedNodes
})
</script>

<style scoped>
ul.k-tree {
  list-style: none;
  padding: 0;
}
</style>
