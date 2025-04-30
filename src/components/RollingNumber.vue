<template>
  <div class="rolling-number">
    <template v-for="(digit, index) in digits" :key="index">
      <RollingDigit
          v-if="typeof digit === 'number'"
          :digit="digit"
          :digitHeight="digitHeight"
          :direction="direction"
          :duration="duration"
      />
      <span v-else class="non-digit">{{ digit }}</span>
    </template>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';
import RollingDigit from './RollingDigit.vue';

const props = withDefaults(
    defineProps<{
      value: number;
      digitHeight?: number;
      direction?: 'up' | 'down';
      duration?: number;
    }>(),
    {
      digitHeight: 40,
      direction: 'up',
      duration: 600,
    }
);

// 안전하게 숫자 및 쉼표 분리
function formatToDigitArray(val: number): (number | string)[] {
  const str = val.toLocaleString('en-US');
  return Array.from(str).map((char) => (/\d/.test(char) ? Number(char) : char));
}

const digits = computed(() => formatToDigitArray(props.value));
</script>

<style scoped>
.rolling-number {
  display: flex;
  align-items: flex-end;
  gap: 2px;
}

.non-digit {
  font-size: 32px;
  font-weight: bold;
  padding: 0 2px;
}
</style>