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

const digits = computed(() => {
  return String(props.value)
      .padStart(1, '0')
      .split('')
      .map((d) => (/\d/.test(d) ? Number(d) : d));
});
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
}
</style>