<script lang="ts" setup>
import Konva from 'konva';
import { onMounted } from 'vue';
import { canvas, stage, layer } from './viewport'
import { activeTool } from '@/tool/tool.model';

const initStage = ({width, height}:{width: number, height: number}) => {
  if(!canvas.value) return
  stage.value = new Konva.Stage({
    container: canvas.value,
    width,
    height,
  })
}
const initLayer = () => {
  if(!stage.value) return
  const newLayer = new Konva.Layer()
  newLayer.getContext().getCanvas()._canvas.style.backgroundColor = '#ffffff'
  layer.value = newLayer
  stage.value?.add(newLayer)
}

const initCanvas = () => {
  if(!canvas.value) return
  initStage({
    width: 500,
    height: 500
  })
  canvas.value!.style.backgroundColor = '#435585'
  initLayer()
}
onMounted(() => {
  initCanvas()
})

defineExpose({
  initCanvas,
  canvas,
  stage,
  layer
})
</script>

<template>
    <div 
      @mousedown="activeTool?.startDraw"
      class="canvas" 
      :ref="(el) => canvas = el as HTMLDivElement" 
      id="canvas-container"></div>
</template>
<style scoped>
.canvas {
  width: 100vw;
  display: flex;
  height: 100%;
  align-items: center;
  justify-content: center;
}
</style>