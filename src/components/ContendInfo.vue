<template>
  <div>
    <p class="text-lg"><slot></slot></p>
    <ol>
      <li v-for="(convert, key) in objectInfo" :key="key">
        {{ key }}<span class="mx-3"> {{ convert }}</span>
      </li>
    </ol>
  </div>
</template>
<script setup>
defineProps({
  objectInfo: Array,
});
</script>
