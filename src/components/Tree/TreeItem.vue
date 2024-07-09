<template>
  <li :class="['k-tree-li', `k-tree-level-${props.level}`]">
    <span :class="[isParent? status ? 'open': 'close' : '', 'trigger-btn']" @click="changeStatus"></span>
    <input type="checkbox" v-show="options.showCheckBox" v-model="checked">
    <div class="k-tree-li-content">
      <slot><span>{{ node.name }}</span></slot>
    </div>
    <div v-show="status"  class="k-tree-subtree">
      <ul v-if="isParent">
        <template v-for="(node, index) in children" :key="index">
          <TreeItem :node="node" :options="props.options" :level="props.level" @change-check="changeChild"/>
        </template>
      </ul>
    </div>
  </li>
</template>

<script setup>
import { ref, watch } from 'vue'
import { defaultOptions } from "./utils/util"
import TreeItem from './TreeItem.vue'

const props = defineProps({
  node: {
    type: Object,
    default: () => ({
      id: 0,
      name: 'tree-node',
      checked: false
    })
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

const emit = defineEmits(['changeCheck'])

const options = ref(props.options)
const node = ref(props.node)
const isParent = ref(node.value?.children?.length)
const children = ref(node.value.children)
const status = ref(options.status || true)
const checked = ref(props.node.checked)

const changeStatus = () => {
  status.value = !status.value
}

watch(() => checked.value, (val) => {
  emit('changeCheck', val, node.value.id)
})

watch(() => props.node, (val) => {
  checked.value = val.checked
}, {
  deep: true
})

const changeChild = (status, id) => {
  emit('changeCheck', status, id)
}
</script>

<style scoped>
li.k-tree-li {
  list-style: none;
  margin: 5px 0;
}

.k-tree-li .trigger-btn {
  cursor: pointer;
  display: inline-block;
  width: 10px;
}

.k-tree-li .trigger-btn.close::after {
  content: '+';
}

.k-tree-li .trigger-btn.open::after {
  content: '-';
}

.k-tree-li-content {
  display: inline-block;
  margin-left: 5px;
}

.k-tree-subtree ul {
  padding: 0 0 0 18px;
}
</style>
