<script setup lang="ts">
import { ref, onMounted } from 'vue'
import RollingNumber from './components/RollingNumber.vue'

const value = ref<number>(0);

onMounted(() => {
  setInterval(() => {
    value.value = Math.floor(Math.random() * 10000);
  }, 2000);
});
</script>

<template>
  <RollingNumber :number="value" :minDigits="1" />
</template>

