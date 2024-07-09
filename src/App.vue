<template>
  <Tree :nodes="nodes" ref="tree"></Tree>


  <button @click="getSelect">获取选中节点</button>
</template>

<script setup>
import { ref } from 'vue'
import Tree from "@/components/Tree"


const tree = ref(null)
const nodes = ref([{
  id: 0,
  name: 'root',
  children: [{
    id: 1,
    name: 'li-1',
    checked: false,
    children: [{
      id: 3,
      name: 'li-1-1',
      checked: false
    }]
  },
  {
    id: 2,
    name: 'li-2',
    checked: false
  }]
}])

const getSelect = () => {
  console.log(tree.value.getCheckedNodes())
}
</script>