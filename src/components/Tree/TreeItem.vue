<template>
  <li class="k-tree-li">
    <span :class="[isParent? status ? 'open': 'close' : '', 'trigger-btn']" @click="changeStatus"></span>
    <input type="checkbox" v-show="options.showCheckBox" v-model="checked">
    {{ node.name }}
  </li>
  <div v-show="status">
    <Tree v-if="isParent" :nodes="children" />
  </div>
</template>

<script setup>
import Tree from './Tree.vue'
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
const isParent = node.value?.children
const children = ref(node.value.children)
const status = ref(options.status || true)
const checked = ref(false)

const changeStatus = () => {
  status.value = !status.value
}

const getChecked = () => {
  return checked.value
}
</script>

<style scoped>
li.k-tree-li {
  padding: 5px 0;
}

.trigger-btn {
  cursor: pointer;
  display: inline-block;
  width: 10px;
}

.trigger-btn.close::after{
  content: '+';
}

.trigger-btn.open::after{
  content: '-';
}
</style>
