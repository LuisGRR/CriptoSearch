<template>
  <div>
    <h2 class="text-lg"><slot></slot></h2>
    <lo>
      <li
        v-for="dataL in limpiezaDatos(dataLinks)"
        :key="dataL"
        class="w-full truncate"
      >
        <a
          class="underline underline-offset-2 decoration-slate-100 hover:decoration-slate-800"
          :href="dataL"
          target="_blank"
          >{{ dataL }}</a
        >
      </li>
    </lo>
  </div>
</template>

<script setup>
defineProps({
  dataLinks: Array,
});

function limpiezaDatos(OnjectDatos) {
  return OnjectDatos.filter((dato) => dato != "");
}
</script>
