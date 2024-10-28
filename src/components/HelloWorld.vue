<script setup lang="ts">
import { computed, onBeforeMount, ref } from 'vue';
const props = defineProps<{
  msg: string
  cols: number
  rows: number
}>()



const fruit = ref<string[]>(['🍎', '🍌', '🍇', '🍊', '🍉', '🍓'])
const items = Array.from({ length: props.cols * props.rows }, (_, i) => ({ symbol: fruit.value[i % fruit.value.length], speed: Math.random() * 10.1 }))

const previusTime = ref(new Date().getTime() * Math.random())
function step(timestamp: number) {

  const elapsed = timestamp - previusTime.value;
  previusTime.value = timestamp;
  requestAnimationFrame(step);
}

onBeforeMount(() => {
  console.log('HelloWorld component is about to mount')
  setInterval(() => {
    // items.value = items.value.slice(1).concat(items.value[0])
  }, 1000)
  requestAnimationFrame(step);
})

const offset = Math.random() * 0.1

const gridStyle = computed(() => {
  return {
    'grid-template-columns': `repeat(${props.cols}, 1fr)`,
    'grid-template-rows': `repeat(${props.rows}, 1fr)`,
  }
})

const style = computed(() => {
  return {
    transform: `rotateY(${previusTime.value * offset}deg)`
  }
})
</script>

<template>
  <div class="grid" :style="gridStyle">

    <div class="grid-item" v-for="(item, index) in items" :key="item.speed" :style="{
      //transform: `rotateY(${previusTime * item.speed}deg)`,
      animationDuration: `${item.speed}s`,
      animationDelay: `${item.speed}s`,
      backgroundSize: `${cols * 100}% ${rows * 100}%`,
      backgroundPosition: `${(index % cols) * (1 / (cols - 1) * 100)}% ${(Math.floor(index / cols)) * (1 / (rows - 1) * 100)}%`
    }">

    </div>
  </div>
</template>

<style scoped>
.bg {
  background-image: url('/src/assets/EBU_TEST_16X9.jpg');
  background-size: cover;
}

.grid {
  display: grid;
  place-items: stretch;
  width: 100%;
  aspect-ratio: 16 / 9;
  font-size: 2rem;
}

.grid-item {
  display: flex;
  height: 100%;
  width: 100%;
  background-color: #4b6394;
  border-radius: 0;
  background-image: url('/src/assets/EBU_TEST_16X9.jpg');
  background-size: 300% 200%;
  background-origin: content-box;
  animation: grid 10s linear 1;
  backface-visibility: hidden;
  transform-style: preserve-3d;
}

.grid-animation {
  animation: grid 10s linear infinite;
}

@keyframes grid {
  0% {
    transform: rotateY(0);
  }

  100% {
    transform: rotateY(180deg);
  }
}
</style>
