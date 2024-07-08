<template>
  <li :class="['k-tree-li', `k-tree-level-${props.level}`]">
    <span :class="[isParent? status ? 'open': 'close' : '', 'trigger-btn']" @click="changeStatus"></span>
    <input type="checkbox" v-show="options.showCheckBox" v-model="checked">
    <slot><span>{{ node.name }}</span></slot>
  </li>
  <template v-show="status">
    <ul v-if="isParent">
      <template v-for="(node, index) in children" :key="index">
        <TreeItem :node="node" :options="props.options" :level="props.level"/>
      </template>
    </ul>
  </template>
</template>

<script setup>
import { ref, watch } from 'vue'
import defineOptions from './Options'
import TreeItem from "./TreeItem.vue"

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
  },
  level: {
    type: Number,
    default: 0
  }
})

const emit = defineEmits(['changeCheck'])

const options = ref(props.options)
const node = ref(props.node)
const isParent = node.value?.children
const children = ref(node.value.children)
const status = ref(options.status || true)
const checked = ref(false)

const changeStatus = () => {
  status.value = !status.value
}

watch(() => checked.value, () => {
  emit('changeCheck', checked.value)
})
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

.trigger-btn.close::after {
  content: '+';
}

.trigger-btn.open::after {
  content: '-';
}
</style>
