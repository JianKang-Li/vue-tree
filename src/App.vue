<template>
  <Tree :nodes="nodes"></Tree>
</template>

<script setup>
import { ref } from 'vue'
import Tree from "@/components/Tree"

const nodes = ref([{
  id: 0,
  name: 'root',
  children: [{
    id: 1,
    name: 'li-1',
    children: [{
      id: 3,
      name: 'li-1-1'
    }]
  },
  {
    id: 2,
    name: 'li-2'
  }]
}])
</script>