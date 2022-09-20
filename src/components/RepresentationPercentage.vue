<template>
  <span :class="[dataPercentage > 0 ? 'text-green-500' : 'text-red-500']">{{
    dataPercentage.toFixed(numberZeroIzq)
  }}</span>
</template>
<script setup>
defineProps({
  dataPercentage: Number,
  numberZeroIzq: Number,
});
</script>
