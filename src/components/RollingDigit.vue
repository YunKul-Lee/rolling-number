<template>
  <div class="digit-container" :style="{ height: digitHeight + 'px' }">
    <div
        class="digit-list"
        :style="{
        transform: `translateY(${translateY}px)`,
        transition: animated ? `transform ${duration}ms ease-in-out` : 'none',
      }"
    >
      <div
          v-for="(digit, index) in visibleDigits"
          :key="index"
          class="digit"
          :style="{ height: digitHeight + 'px', lineHeight: digitHeight + 'px' }"
      >
        {{ digit }}
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watch, nextTick } from 'vue';

const props = withDefaults(
    defineProps<{
      digit: number;
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

const currentDigit = ref(props.digit);
const visibleDigits = ref<number[]>([props.digit]);
const translateY = ref(0);
const animated = ref(false);

watch(
    () => props.digit,
    (newDigit, oldDigit) => {
      if (newDigit !== oldDigit) {
        rollTo(newDigit);
      }
    }
);

function getDigitSequence(from: number, to: number, direction: 'up' | 'down'): number[] {
  const result: number[] = [];
  if (direction === 'up') {
    let i = from;
    while (i !== to) {
      result.push(i);
      i = (i + 1) % 10;
    }
  } else {
    let i = from;
    while (i !== to) {
      result.push(i);
      i = (i - 1 + 10) % 10;
    }
  }
  result.push(to);
  return result;
}

async function rollTo(newDigit: number) {
  const from = currentDigit.value;
  const sequence = getDigitSequence(from, newDigit, props.direction);
  visibleDigits.value = sequence;
  currentDigit.value = newDigit;

  animated.value = false;

  const totalHeight = props.digitHeight * (sequence.length - 1);
  translateY.value =
      props.direction === 'down'
          ? -totalHeight
          : 0;

  await nextTick();

  requestAnimationFrame(() => {
    requestAnimationFrame(() => {
      animated.value = true;
      translateY.value =
          props.direction === 'down'
              ? 0
              : -totalHeight;
    });
  });

  // 롤링 후 애니메이션만 제거 (DOM은 그대로)
  setTimeout(() => {
    animated.value = false;
    // NO reset of visibleDigits or translateY
  }, props.duration + 50);
}
</script>

<style scoped>
.digit-container {
  overflow: hidden;
  display: inline-block;
}

.digit-list {
  display: flex;
  flex-direction: column;
  will-change: transform;
}

.digit {
  text-align: center;
  font-size: 32px;
  font-weight: bold;
}
</style>