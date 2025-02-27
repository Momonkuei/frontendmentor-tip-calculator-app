<template>
  <button
    class="rounded-[0.3125rem] max-w-[9.1875rem] flex items-center justify-center text-2xl leading-normal w-full"
    :class="[variants[variant] ?? variants.default]"
    @click="$emit('click', value)"
  >
    <slot></slot>
  </button>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue'
const variants = {
  default: 'bg-[#00474B] text-white  py-[0.375rem] hover:bg-[#26c2AE] hover:text-[#00474B]',
  active: 'bg-[#26C2AE] text-[#00474B] hover:opacity-70',
}

const props = defineProps({
  value: Number,
  variant: {
    type: String,
    default: 'default',
  },
})
const emit = defineEmits(['click'])
</script>
