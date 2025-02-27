<template>
  <div class="flex justify-between items-center font-space-mono">
    <div class="">
      <div class="text-white font-bold text-base">{{ title }}</div>
      <div class="font-bold text-[#7F9D9F] text-[0.8125rem] leading-normal">/ person</div>
    </div>
    <div class="text-[#26C2AE] text-5xl font-bold leading-normal">${{ formattedVal }}</div>
  </div>
</template>

<script setup>
import { computed, watch } from 'vue'
const props = defineProps({
  title: String,
  val: Number,
})

const formattedVal = computed(() => {
  return props.val ? Number(props.val).toFixed(2) : '0.00'
})

// 監聽 props.val 的變化
// watch(
//   () => props.val,
//   (newVal) => {
//     console.log(`val 變了: ${newVal}`)
//   },
// )
</script>
