<template>
  <div class="font-space-mono flex flex-col gap-[0.375rem]">
    <div>
      <div class="text-base font-bold">{{ title }}</div>
      <ErrorMessage name="inputValue" class="text-red-500 text-sm" />
    </div>

    <div class="flex bg-[#F3F9FA] relative">
      <div class="absolute top-1/2 left-[1.25rem] translate-x-[-50%] translate-y-[-50%]">
        <slot></slot>
      </div>
      <!-- 使用 VField 包覆 input，並設定 validateOnBlur -->
      <Field
        class="text-2xl text-[#00474B] font-bold bg-transparent text-end w-full px-4 py-[0.375rem] border-2 border-transparent focus:border-[#26C2AE] outline-none rounded-[0.3125rem]"
        type="text"
        name="inputValue"
        rules="notEmpty"
        v-model="localValue"
      />
      <!-- 顯示錯誤訊息 -->
    </div>
  </div>
</template>

<script setup>
import { ref, watch, defineEmits } from 'vue'
import { Field, ErrorMessage, defineRule } from 'vee-validate'
const props = defineProps({
  title: String,
  modelValue: [String, Number],
})
const emit = defineEmits(['update:modelValue'])

const localValue = ref(props.modelValue)

// 監聽 props.modelValue 的變化，並同步到 localValue
watch(
  () => props.modelValue,
  (newValue) => {
    localValue.value = newValue
  },
)

// 監聽 localValue 的變化，發送事件更新父組件
watch(
  () => localValue.value,
  (newValue) => {
    emit('update:modelValue', newValue)
  },
)
// 驗證函式：檢查輸入不可為空，且不能為 0

defineRule('notEmpty', (value) => {
  if (value === null || value === undefined || value === '' || value.toString().trim() === '') {
    return '數字不可為空'
  }
  // 將 value 轉為數字來檢查
  if (Number(value) === 0) {
    return '輸入不得為 0'
  }
  return true
})
</script>
