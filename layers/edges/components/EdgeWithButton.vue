<script
  setup
  lang="ts"
>
import { BaseEdge, EdgeLabelRenderer, getBezierPath, useVueFlow, type Position } from '@vue-flow/core'

const props = defineProps({
  id: {
    type: String,
    required: true
  },
  sourceX: {
    type: Number,
    required: true
  },
  sourceY: {
    type: Number,
    required: true
  },
  targetX: {
    type: Number,
    required: true
  },
  targetY: {
    type: Number,
    required: true
  },
  sourcePosition: {
    type: String as () => Position,
    required: true
  },
  targetPosition: {
    type: String as () => Position,
    required: true
  },
  markerEnd: {
    type: String,
    required: false,
    default: ''
  },
  style: {
    type: Object,
    required: false,
    default: () => ({})
  }
})

const { removeEdges } = useVueFlow()

const path = computed(() => getBezierPath(props))
</script>

<script lang="ts">
export default {
  inheritAttrs: false
}
</script>

<template>
  <!-- You can use the `BaseEdge` component to create your own custom edge more easily -->
  <BaseEdge
    :id="id"
    :style="style"
    :path="path[0]"
    :marker-end="markerEnd"
  />

  <!-- Use the `EdgeLabelRenderer` to escape the SVG world of edges and render your own custom label in a `<div>` ctx -->
  <EdgeLabelRenderer>
    <div
      :style="{
        pointerEvents: 'all',
        position: 'absolute',
        transform: `translate(-50%, -50%) translate(${path[1]}px,${path[2]}px)`,
      }"
      class="nodrag nopan"
    >
      <button
        class="edgebutton"
        @click="removeEdges(id)"
      >×</button>
    </div>
  </EdgeLabelRenderer>
</template>

<style>
@import '../assets/edges-style.css';
</style>
