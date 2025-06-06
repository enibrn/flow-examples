<script
  setup
  lang="ts"
>
import type { PropType } from 'vue'
import { Position, getBezierPath, getSmoothStepPath, type Node } from '@vue-flow/core'

interface EdgeData {
  pathType: string
}

const props = defineProps({
  id: {
    type: String,
    required: true,
  },
  sourceX: {
    type: Number,
    required: true,
  },
  sourceY: {
    type: Number,
    required: true,
  },
  targetX: {
    type: Number,
    required: true,
  },
  targetY: {
    type: Number,
    required: true,
  },
  sourcePosition: {
    type: String as PropType<Position>,
    required: true,
  },
  targetPosition: {
    type: String as PropType<Position>,
    required: true,
  },
  sourceNode: {
    type: Object as PropType<Node>,
    required: true,
  },
  targetNode: {
    type: Object as PropType<Node>,
    required: true,
  },
  data: {
    type: Object as PropType<EdgeData>,
    required: true,
  },
})

const path = computed(() => {
  if (props.sourceNode && props.targetNode) {
    if (props.data.pathType === 'bezier') {
      if (
        (props.sourcePosition === Position.Bottom && props.targetPosition === Position.Top) ||
        (props.sourcePosition === Position.Top && props.targetPosition === Position.Bottom)
      ) {
        // for horizontal loopback edges
        const radiusX = 60
        const radiusY = props.sourceY - props.targetY

        return [`M ${props.sourceX} ${props.sourceY} A ${radiusX} ${radiusY} 0 1 0 ${props.targetX} ${props.targetY}`]
      } else if (
        (props.sourcePosition === Position.Left && props.targetPosition === Position.Right) ||
        (props.sourcePosition === Position.Right && props.targetPosition === Position.Left)
      ) {
        // for vertical loopback edges
        const radiusX = (props.sourceX - props.targetX) * 0.6
        const radiusY = 50

        return [`M ${props.sourceX} ${props.sourceY} A ${radiusX} ${radiusY} 0 1 0 ${props.targetX} ${props.targetY}`]
      }
    } else if (props.data.pathType === 'smoothstep') {
      let centerX, centerY
      if (props.sourceNode === props.targetNode) {
        if (
          (props.sourcePosition === Position.Bottom && props.targetPosition === Position.Top) ||
          (props.sourcePosition === Position.Top && props.targetPosition === Position.Bottom)) {
          centerX = props.sourceX - 40 - 150 / 2  // Use default width
          centerY = (props.sourceY + props.targetY) / 2
        } else if (
          (props.sourcePosition === Position.Left && props.targetPosition === Position.Right) ||
          (props.sourcePosition === Position.Right && props.targetPosition === Position.Left)
        ) {
          centerX = (props.sourceX + props.targetX) / 2
          centerY = props.sourceY + 40 + 50 / 2  // Use default height
        }
      } return getSmoothStepPath({
        sourceX: props.sourceX,
        sourceY: props.sourceY,
        targetX: props.targetX,
        targetY: props.targetY,
        sourcePosition: props.sourcePosition,
        targetPosition: props.targetPosition,
        centerX,
        centerY,
      })
    }
  }

  // default to bezier path
  return getBezierPath(props)
})

defineOptions({
  inheritAttrs: false,
})
</script>

<template>
  <path
    :d="path[0]"
    fill="none"
    stroke="black"
  />
</template>
