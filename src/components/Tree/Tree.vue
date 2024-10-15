<template>
  <ul ref="el" class="k-tree">
    <div v-for="node in nodes" :key="node.id">
      <TreeItem :node="node" :options="options" :level="level" @change-check="changeItemChecked"/>
    </div>
  </ul>
</template>

<script setup>
import { ref, computed } from 'vue'
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

const changeItem = (arr, id, node) => {
  for (let i = 0; i < arr.length; i++) {
    if (arr[i].id === id) {
      for(const key in node) {
        arr[i][key] = node[key]
      }
    }

    if (arr[i].children && arr[i].children.length) {
      changeItem(arr[i].children, id, node)
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

const changeItemChecked = (status, id) => {
  changeItem(nodes.value, id, {checked: status})
}

const changeNode = (id, op) => {
  changeItem(nodes.value, id, op)
}

const getCheckedNodes = () => {
  return getNodes(nodes.value, 'checked' , true)
}

const getNodesByParams = (key, value) => {
  return getNodes(nodes.value, key, value)
}

const getAllNodes = () => {
  return getNodes(nodes.value)
}

defineExpose({
  getCheckedNodes,
  getNodesByParams,
  changeItemChecked,
  changeNode,
  getAllNodes
})
</script>

<style scoped>
ul.k-tree {
  list-style: none;
  padding: 0;
}
</style>
