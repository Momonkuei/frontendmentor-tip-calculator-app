<template>
  <Form class="w-full flex flex-col gap-10" @submit.prevent>
    <!-- Bill -->
    <div class="font-space-mono flex flex-col gap-[0.375rem]">
      <div class="flex items-center justify-between">
        <div class="text-base font-bold">Bill</div>
        <ErrorMessage name="billValue" class="text-red-500 text-sm" />
      </div>

      <div class="flex bg-[#F3F9FA] relative">
        <div class="absolute top-1/2 left-[1.25rem] translate-x-[-50%] translate-y-[-50%]">
          <img :src="IconDollar" alt="" />
        </div>
        <!-- 使用 VField 包覆 input，並設定 validateOnBlur -->
        <Field
          class="text-2xl text-[#00474B] font-bold bg-transparent text-end w-full px-4 py-[0.375rem] border-2 border-transparent focus:border-[#26C2AE] outline-none rounded-[0.3125rem]"
          type="text"
          name="billValue"
          rules="notEmpty"
          v-model="bill"
        />
        <!-- 顯示錯誤訊息 -->
      </div>
    </div>

    <!-- Tip -->
    <div class="font-space-mono flex flex-col gap-[1rem]">
      <div class="text-base font-bold">Select Tip %</div>
      <div class="grid grid-cols-3 gap-x-[0.875rem] gap-y-4">
        <round-button
          v-for="rate in [5, 10, 15, 25, 50]"
          :key="rate"
          :variant="selectedTip === rate ? 'active' : ''"
          @click="setTip(rate)"
        >
          {{ rate }}%
        </round-button>
        <Field
          type="text"
          placeholder="Custom"
          class="bg-[#F3F9FA] text-2xl text-[#00474B] font-bold text-end w-full px-4 py-[0.375rem] border-2 border-transparent focus:border-[#26C2AE] outline-none rounded-[0.3125rem] placeholder:text-[#00474B]"
          v-model="validatedTipRate"
          name="localTipRate"
        />
      </div>
    </div>

    <!-- PeoPle -->
    <div class="font-space-mono flex flex-col gap-[0.375rem]">
      <div class="flex items-center justify-between">
        <div class="text-base font-bold">Number of People</div>
        <ErrorMessage name="peopleValue" class="text-red-500 text-sm" />
      </div>

      <div class="flex bg-[#F3F9FA] relative">
        <div class="absolute top-1/2 left-[1.25rem] translate-x-[-50%] translate-y-[-50%]">
          <img :src="IconPerson" alt="" />
        </div>
        <!-- 使用 VField 包覆 input，並設定 validateOnBlur -->
        <Field
          class="text-2xl text-[#00474B] font-bold bg-transparent text-end w-full px-4 py-[0.375rem] border-2 border-transparent focus:border-[#26C2AE] outline-none rounded-[0.3125rem]"
          type="text"
          name="peopleValue"
          rules="notEmpty"
          v-model="people"
        />
        <!-- 顯示錯誤訊息 -->
      </div>
    </div>
  </Form>
</template>

<script setup>
import { inject, ref, watch, computed } from 'vue'
import { Form, Field, ErrorMessage, defineRule, useForm } from 'vee-validate'
import RoundButton from '@/components/RoundButton.vue'

import IconDollar from '@/assets/images/icon-dollar.svg?url'
import IconPerson from '@/assets/images/icon-person.svg?url'

const bill = inject('bill')
const people = inject('people')
const empty = inject('empty')
const inputTipRate = ref(null)

const { setFieldError, validateField } = useForm() // 取得 resetForm 方法

const props = defineProps({
  tipRate: Number,
})
const emit = defineEmits(['update:tipRate'])

const selectedTip = ref(props.tipRate) // 記錄當前選中的 Tip %

const setTip = (rate) => {
  selectedTip.value = rate
  emit('update:tipRate', rate)
}

watch(empty, async (newVal) => {
  console.log('restFormAction', empty.value)
  if (newVal) {
    inputTipRate.value = null
    empty.value = false

    // **清除錯誤訊息**
    setFieldError('localTipRate', '')
  }
})
// 監聽 props.tipRate 變化，確保外部變更時更新 selectedTip
watch(
  () => props.tipRate,
  (newRate) => {
    selectedTip.value = newRate
  },
)

watch(inputTipRate, (newVal) => {
  if (newVal) {
    selectedTip.value = null
    emit('update:tipRate', newVal)
  }
})

defineRule('notEmpty', (value) => {
  if (value === null || value === undefined || value === '' || value.toString().trim() === '') {
    return '數字不可為空'
  }
  // 將 value 轉為數字來檢查
  if (Number(value) === 0) {
    return '輸入不得為 0'
  }

  if (!Number(value)) {
    return '請輸入數字'
  }
  return true
})

const validatedTipRate = computed({
  get: () => inputTipRate.value ?? '', // 確保 null 時回傳空字串，讓 UI 正確顯示 Custom
  set: (value) => {
    let num = parseFloat(value)

    // 若不是數字，則清空輸入
    if (isNaN(num) || num <= 0 || num > 100) {
      inputTipRate.value = undefined // 或者改成 ''，確保 UI 更新
    } else {
      inputTipRate.value = num
    }
  },
})
</script>
