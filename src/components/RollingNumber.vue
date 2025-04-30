<template>
  <div class="rolling-number">
    <template v-for="(char, index) in formattedDigits" :key="index">
      <RollingDigit
          v-if="char !== ','"
          :digit="parseInt(char)"
          :digitHeight="digitHeight"
          :direction="direction"
          :duration="duration"
      />
      <span v-else class="separator">,</span>
    </template>
  </div>
</template>

<script setup lang="ts">
import { computed, ref, watch } from 'vue';
import RollingDigit from './RollingDigit.vue';

const props = withDefaults(
    defineProps<{
      number: number;
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

const previous = ref(props.number);

// 숫자 자릿수 변경을 제대로 추적하도록 수정
const paddedNumber = computed(() => {
  // 부호 제거하고, 숫자 길이 맞추기
  const currentStr = Math.abs(props.number).toString();
  const prevStr = Math.abs(previous.value).toString();

  // 자릿수 차이를 맞추기 위해 길이를 맞추는 방식
  const maxLength = Math.max(currentStr.length, prevStr.length);

  return currentStr.padStart(maxLength, '0');
});

// 쉼표 삽입 및 숫자 형식 유지
const formattedDigits = computed(() => {
  const numWithoutCommas = paddedNumber.value.split('');
  const result: string[] = [];

  for (let i = 0; i < numWithoutCommas.length; i++) {
    const posFromRight = numWithoutCommas.length - i - 1;
    result.push(numWithoutCommas[i]);

    // 3자리마다 쉼표 삽입
    if (posFromRight > 0 && posFromRight % 3 === 0) {
      result.push(',');
    }
  }

  return result;
});

watch(() => props.number, (newVal) => {
  previous.value = newVal;
});
</script>

<style scoped>
.rolling-number {
  display: flex;
  align-items: center;
  font-family: monospace;
}

.separator {
  display: inline-block;
  width: 0.5em;
  text-align: center;
  font-size: 32px;
  font-weight: bold;
}
</style>