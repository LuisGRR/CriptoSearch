<script setup>
import { ref } from "vue";
import { onClickOutside } from "@vueuse/core";
import RepresenationPercentage from "./RepresentationPercentage.vue";
import SelectdCopy from "./SelectdCopy.vue";
import ContendEnlaces from "./ContendEnlces.vue";
// import ContendInfo from "./ContendInfo.vue";

defineExpose({
  openModal,
});

const open = ref(false);
const modal = ref(null);
const dataBitcoint = ref(null);

function openModal(idBitcoin) {
  open.value = true;
  consultaCripto(idBitcoin)
    .then((result) => (dataBitcoint.value = result))
    .catch();
}

onClickOutside(modal, () => {
  cerrarModal();
});

function cerrarModal() {
  open.value = false;
  dataBitcoint.value = null;
}

async function consultaCripto(id) {
  const res = await fetch(`https://api.coingecko.com/api/v3/coins/${id}`);
  if (res.status == 200) {
    const data = await res.json();
    return data;
  }
  throw new Error(res.status);
}

function limpiezaDatos(OnjectDatos) {
  return OnjectDatos.filter((dato) => dato != "");
}
</script>

<template>
  <Teleport to="body">
    <Transition name="modal">
      <section
        v-if="open"
        class="fixed top-0 left-0 w-screen h-screen bg-slate-800/50 flex justify-center items-center"
      >
        <div class="bg-white rounded-md shadow-xl w-4/5 h-4/5" ref="modal">
          <div class="fixed ml-2 mt-2">
            <button @click="cerrarModal">
              <img src="../assets/x-svgrepo-com.svg" alt="" />
            </button>
          </div>
          <div
            class="flex flex-col w-full h-full space-y-1 px-2"
            v-if="dataBitcoint"
          >
            <div class="basis-1/4 flex space-x-5 mx-1">
              <div class="basis-6/12">
                <div class="w-full flex flex-col items-center">
                  <img
                    :src="dataBitcoint.image.large"
                    class="w-40 h-40 my-5"
                    alt=""
                  />
                  <SelectdCopy
                    :data-bitcointplatforms="dataBitcoint.platforms"
                  />
                </div>
              </div>
              <div class="basis-9/12">
                <div class="flex-col w-full">
                  <div class="flex p-5 space-x-5 justify-center">
                    <h2 class="text-6xl">{{ dataBitcoint.name }}</h2>
                    <span class="text-4xl mt-5 text-slate-300">{{
                      dataBitcoint.symbol
                    }}</span>
                  </div>
                  <div class="flex flex-col">
                    <div class="flex justify-evenly space-x-10">
                      <p class="w-32 text-lg tracking-wide">
                        high
                        <span class="ml-1">{{
                          dataBitcoint.market_data.high_24h.usd
                        }}</span>
                      </p>
                      <p class="w-32 text-lg tracking-wide">
                        low
                        <span class="ml-1">
                          {{ dataBitcoint.market_data.low_24h.usd }}</span
                        >
                      </p>
                    </div>
                    <div class="my-3">
                      <h2 class="text-center text-3xl">
                        Price change percentage
                      </h2>
                    </div>
                    <div class="flex">
                      <div
                        class="basis-2/4 flex flex-col space-y-2 text-center"
                      >
                        <p class="text-lg">
                          24h
                          <RepresenationPercentage
                            :data-percentage="
                              dataBitcoint.market_data
                                .price_change_percentage_24h
                            "
                            :number-zero-izq="2"
                            class="ml-5"
                          />
                        </p>
                        <p class="text-lg">
                          7d
                          <RepresenationPercentage
                            :data-percentage="
                              dataBitcoint.market_data
                                .price_change_percentage_7d
                            "
                            :number-zero-izq="2"
                            class="ml-5"
                          />
                        </p>
                      </div>
                      <div
                        class="basis-2/4 flex flex-col space-y-2 text-center"
                      >
                        <!-- <p class="text-lg">
                          24h in currency
                          <RepresenationPercentage
                            :data-percentage="
                              dataBitcoint.market_data
                                .price_change_percentage_24h_in_currency.usd
                            "
                            :number-zero-izq="2"
                            class="ml-5"
                          />
                        </p>
                        <p class="text-lg">
                          7d in currency
                          <RepresenationPercentage
                            :data-percentage="
                              dataBitcoint.market_data
                                .price_change_percentage_7d_in_currency.usd
                            "
                            :number-zero-izq="2"
                            class="ml-5"
                          />
                        </p> -->
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="basis-1/4 flex py-1 h-full">
              <div
                class="basis-2/4 px-2 flex flex-col space-y-2 overflow-auto scroll-auto h-56"
              >
                <ContendEnlaces :data-links="dataBitcoint.links.homepage"
                  >HomePage</ContendEnlaces
                >
                <ContendEnlaces :data-links="dataBitcoint.links.blockchain_site"
                  >Block chain site</ContendEnlaces
                >
                <div>
                  <p class="text-lg">Categories</p>
                  <ol>
                    <li
                      v-for="categories in limpiezaDatos(
                        dataBitcoint.categories
                      )"
                      :key="categories"
                    >
                      {{ categories }}
                    </li>
                  </ol>
                </div>
              </div>
              <div
                class="basis-2/4 p-2 flex flex-col space-y-5 overflow-auto scroll-auto h-56"
              >
                <div
                  class=""
                  v-for="tickers in dataBitcoint.tickers"
                  :key="tickers.BTC"
                >
                  <div class="flex space-x-4">
                    <p class="basis-1/4 text-lg truncate">{{ tickers.base }}</p>
                    <p class="basis-1/5 text-lg truncate">
                      {{ tickers.target }}
                    </p>
                    <p class="basis-1/3 text-lg">{{ tickers.market.name }}</p>
                    <p class="basis-1/4 text-lg">
                      <a
                        class="underline underline-offset-2 decoration-slate-300 hover:decoration-slate-800"
                        :href="tickers.trade_url"
                        target="_blank"
                        >Trade site</a
                      >
                    </p>
                  </div>
                  <div class="flex space-x-2">
                    <div class="basis-1/2">
                      <!-- <ContendInfo :object-info="tickers.converted_volume"
                        >Converted volume</ContendInfo
                      > -->

                      <div>
                        <p class="text-lg">Converted volume</p>
                        <ol>
                          <li
                            v-for="(convertV, key) in tickers.converted_volume"
                            :key="key"
                          >
                            {{ key }}<span class="mx-3"> {{ convertV }}</span>
                          </li>
                        </ol>
                      </div>
                    </div>
                    <div class="basis-1/2">
                      <!-- <ContendInfo :object-info="tickers.converted_volume"
                        >Converted last</ContendInfo
                      > -->
                      <p class="text-lg">Converted last</p>
                      <ol class="">
                        <li
                          v-for="(convertV, key) in tickers.converted_last"
                          :key="key"
                        >
                          {{ key }}<span class="mx-3">{{ convertV }}</span>
                        </li>
                      </ol>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>
    </Transition>
  </Teleport>
</template>

<style>
.modal-enter-active,
.modal-leave-active {
  transition: all 0.25s ease;
}
.modal-enter-from,
.modal-leave-to {
  opacity: 0;
  transform: scale(1.1);
}
</style>
