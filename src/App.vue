<template>
  <div class="flex justify-center items-center h-screen w-screen flex-col">
    <div class="mb-[5rem]">
      <img src="../src/assets/images/logo.svg" alt="" />
    </div>
    <div class="container max-w-[57.5rem] bg-white rounded-[1.5625rem] p-8 flex gap-[3rem]">
      <left-box :tipRate="tipRate" @update:tipRate="(value) => (tipRate = value)"></left-box>
      <div class="w-full bg-[#00474B] rounded-[0.9375rem] p-10">
        <div class="flex flex-col justify-between h-full">
          <div>
            <PriceLabelCard title="Tip Amount" :val="tipAmount"></PriceLabelCard>
            <PriceLabelCard title="Total" :val="totalForPerson"></PriceLabelCard>
          </div>
          <button
            class="py-[0.5625rem] bg-[#26C2AE] w-full text-[#00474B] rounded-[0.3125rem] hover:bg-[#9FE8DF] text-xl font-space-mono leading-normal font-bold"
            @click="rest"
          >
            RESET
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, provide, watch } from 'vue'
import LeftBox from '@/components/LeftBox.vue'
import PriceLabelCard from '@/components/PriceLabelCard.vue'

const bill = ref(null)
const people = ref(null)
const tipRate = ref(0)
const totalResult = ref(0)
const tipAmount = ref(0)
const totalForPerson = ref(0)
const empty = ref(true)

provide('bill', bill)
provide('people', people)
provide('tipRate', tipRate)
provide('empty', empty)

const updateResult = () => {
  if (bill.value > 0 && people.value > 0 && tipRate.value >= 0) {
    let tip = Number(bill.value * (tipRate.value / 100))
    totalResult.value = ((Number(bill.value) + tip) / people.value).toFixed(2)
    tipAmount.value = (tip / people.value).toFixed(2)
    totalForPerson.value = totalResult.value
    empty.value = false
  }
}

const rest = () => {
  bill.value = 0
  people.value = 0
  tipRate.value = 0
  totalResult.value = 0
  tipAmount.value = 0
  totalForPerson.value = 0
  empty.value = true
}

watch([bill, people, tipRate], () => {
  updateResult()
})
</script>
