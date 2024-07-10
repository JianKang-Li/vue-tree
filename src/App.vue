<template>
  <Tree :nodes="nodes" ref="tree"></Tree>


  <button @click="getSelect">获取选中节点</button>
  <button @click="cancelSelect">取消选中</button>
  <button @click="getID1">获取id为1的节点</button>
  <button @click="showNode">展示节点</button>
  <button @click="getAll">获取所有节点</button>
</template>

<script setup>
import { ref } from 'vue'
import Tree from "@/components/Tree"


const tree = ref(null)
const nodes = ref([{
  id: 0,
  name: 'root',
  checked: true,
  children: [{
    id: 1,
    name: 'li-1',
    checked: false,
    children: [{
      id: 3,
      name: 'li-1-1',
      checked: false,
      visible: false
    }]
  },
  {
    id: 2,
    name: 'li-2',
    checked: false
  }]
}])

const cancelSelect = () => {
  tree.value.changeItemChecked(false, 0)
}

const getID1 = () => {
  const nodes = tree.value.getNodesByParams('id', 1)
  console.log(nodes)
}

const showNode = () => {
  tree.value.changeNode(3, {visible: true})
}

const getSelect = () => {
  console.log(tree.value.getCheckedNodes())
}

const getAll = () => {
  console.log(tree.value.getAllNodes())
}
</script>