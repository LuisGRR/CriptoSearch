<template>
  <div v-if="!isEmpty(dataBitcointplatforms)" class="flex p-1">
    <div>
      <select v-model="selectd" class="w-40">
        <option
          v-for="(platforms, key) in CleanObject(dataBitcointplatforms)"
          :key="key"
          :value="platforms"
        >
          {{ key }}
        </option>
      </select>
    </div>
    <div class="w-44 mx-1">
      <p class="truncate">
        {{ selectd }}
      </p>
    </div>
    <div>
      <button @click="copy(selectd)">
        <img
          class="w-5 h-5 hover:scale-95"
          src="../assets/copy-svgrepo-com.svg"
          alt=""
        />
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { useClipboard } from "@vueuse/core";

defineProps({
  dataBitcointplatforms: Object,
});

const { copy } = useClipboard();

const selectd = ref("");

function CleanObject(ObjectC) {
  if (Object.keys(ObjectC).some((k) => k != "" && k != " ")) {
    delete ObjectC[""];
    delete ObjectC[" "];
    return ObjectC;
  }
}

function isEmpty(value) {
  var isEmptyObject = function (a) {
    if (typeof a.length === "undefined") {
      // it's an Object, not an Array
      var hasNonempty = Object.keys(a).some(function nonEmpty(element) {
        return !isEmpty(a[element]);
      });
      return hasNonempty ? false : isEmptyObject(Object.keys(a));
    }

    return !a.some(function nonEmpty(element) {
      // check if array is really not empty as JS thinks
      return !isEmpty(element); // at least one element should be non-empty
    });
  };
  return (
    value == false ||
    typeof value === "undefined" ||
    value == null ||
    (typeof value === "object" && isEmptyObject(value))
  );
}
</script>
