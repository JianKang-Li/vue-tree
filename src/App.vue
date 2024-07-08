<template>
  <Tree :nodes="nodes"></Tree>
</template>

<script setup>
import { ref } from 'vue'
import Tree from "@/components/Tree"

const nodes = ref([{
  id: 1,
  isParent: true,
  name: 'root'
},
{
  id: 2,
  isParent: true,
  name: 'li1'
},
{
  id: 3,
  isParent: false,
  name: 'child'
}])
</script>