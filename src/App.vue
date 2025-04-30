<script setup lang="ts">
import RollingNumber from './components/RollingNumber.vue';
import { ref, onMounted } from 'vue';

const number = ref(0);

onMounted(() => {
  setInterval(() => {
    number.value = Math.floor(Math.random() * 10000);
  }, 2000);
});
</script>

<template>
  <RollingNumber :number="number" direction="down" :digitHeight="40" />
  {{ number }}
</template>

