<script setup lang="ts">
import RollingNumber from './components/RollingNumber.vue';
import { ref, onMounted } from 'vue';

const number = ref(0);

// 천 단위 쉼표 포함 포맷
const formatNumber = (val: string | number) =>
    val.toLocaleString('en-US', { minimumFractionDigits: 0 });

onMounted(() => {
  setInterval(() => {
    number.value = Math.floor(Math.random() * 100000);
  }, 2000);
});
</script>

<template>
  <RollingNumber :value="number" direction="down" :digitHeight="40" />
  {{ number }}
</template>

