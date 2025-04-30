<script setup lang="ts">
import { computed } from 'vue'
import RollingDigit from './RollingDigit.vue'

const props = defineProps<{
  number: number
  minDigits?: number
  digitHeight?: number
}>();

const digitHeight = props.digitHeight ?? 40
const minDigits = props.minDigits ?? 1

const digits = computed<number[]>(() => {
  const str = props.number.toString().padStart(minDigits, '0')
  return [...str].map((ch) => parseInt(ch, 10))
});
</script>

<template>
  <div class="rolling-number">
    <RollingDigit
        v-for="(d, index) in digits"
        :key="index"
        :digit="d"
        :digitHeight="digitHeight"
    />
  </div>
</template>

<style scoped>
.rolling-number {
  display: flex;
}
</style>
