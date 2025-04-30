<script setup lang="ts">
import { defineProps } from 'vue'

const props = defineProps<{
  digit: number
  digitHeight?: number
}>();

const digitHeight = props.digitHeight ?? 40
</script>

<template>
  <div class="digit-wheel" :style="{ height: digitHeight + 'px' }">
    <div
        class="digit-list"
        :style="{
        transform: `translateY(-${digitHeight * digit}px)`,
        transition: 'transform 0.5s ease-in-out',
      }"
    >
      <div
          v-for="n in 10"
          :key="n"
          class="digit"
          :style="{ height: digitHeight + 'px' }"
      >
        {{ (n - 1 + 10) % 10 }}
      </div>
    </div>
  </div>
</template>

<style scoped>
.digit-wheel {
  overflow: hidden;
  width: 30px;
  display: inline-block;
}
.digit-list {
  display: flex;
  flex-direction: column;
}
.digit {
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 32px;
}
</style>