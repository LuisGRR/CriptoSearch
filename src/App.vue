<script>
import modalCripto from "@/components/ModalCripto.vue";
export default {
  name: "App",
  components: {
    modalCripto,
  },
  data() {
    return {
      coins: [],
      filteredCoin: [],
      titles: [
        "#",
        "Coin",
        "Price",
        "Price Change",
        "24 Volumne",
        "7d percentage",
      ],
      textSearch: "",
      open: null,
    };
  },
  async mounted() {
    const res = await fetch(
      "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false&price_change_percentage=1h%2C24h%2C7d"
    );
    const data = await res.json();
    // console.log(data);
    this.coins = data;
    this.filteredCoin = data;
  },
  methods: {
    searchCoin() {
      this.filteredCoin = this.coins.filter(
        (coin) =>
          coin.name
            .toLowerCase()
            .includes(this.textSearch.toLocaleLowerCase()) ||
          coin.symbol
            .toLowerCase()
            .includes(this.textSearch.toLocaleLowerCase())
      );
    },
    showModalCripto(id) {
      // this.openModal = true;
      // console.log("hello");
      // console.log(this.openModal);
      this.$refs.modal.openModal(id);
    },
  },
};
</script>

<template>
  <header class="flex flex-col my-5 mx-10 space-y-5">
    <div class="text-center">
      <h1 class="text-3xl font-bold">Cripto Search</h1>
    </div>
    <div>
      <label class="flex">
        <span class="left-0 flex items-center pl-2">
          <svg class="h-5 w-5 fill-slate-600" viewBox="0 0 20 20">
            <path
              fill-rule="evenodd"
              d="M8 4a4 4 0 100 8 4 4 0 000-8zM2 8a6 6 0 1110.89 3.476l4.817 4.817a1 1 0 01-1.414 1.414l-4.816-4.816A6 6 0 012 8z"
              clip-rule="evenodd"
            ></path>
          </svg>
        </span>
        <input
          class="border-b border-slate-200 placeholder:italic placeholder:text-slate-400 focus:outline-none focus:border-b focus:border-slate-800"
          type="text"
          placeholder="Search coin or symbol"
          @keyup="searchCoin()"
          v-model="textSearch"
        />
      </label>
    </div>
  </header>

  <main>
    <modalCripto ref="modal"></modalCripto>

    <table class="border-collapse table-auto w-full mt-5 px-5 text-center">
      <thead class="border-b border-slate-200">
        <tr>
          <th v-for="title in titles" :key="title">
            {{ title }}
          </th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="(coin, index) in filteredCoin"
          :key="coin.id"
          class="border-b border-slate-200 hover:bg-slate-700 hover:text-white"
          @click="showModalCripto(coin.id)"
        >
          <td class="border-r border-slate-200">
            {{ index + 1 }}
          </td>
          <td>
            <div class="flex space-x-5 p-4 pl-8 items-center">
              <img class="w-6 h-6" :src="coin.image" :alt="coin.name" />
              <span>{{ coin.name }}</span>
              <span class="text-slate-300">{{ coin.symbol }}</span>
            </div>
          </td>
          <td>${{ coin.current_price }}</td>
          <td
            :class="[
              coin.price_change_percentage_24h > 0
                ? 'text-green-500'
                : 'text-red-500',
            ]"
          >
            {{ coin.price_change_percentage_24h.toFixed(1) }}%
          </td>
          <td>${{ coin.total_volume.toLocaleString() }}</td>
          <!-- <td
            :class="[
              coin.price_change_percentage_1h_in_currency > 0
                ? 'text-green-500'
                : 'text-red-500',
            ]"
          >
            {{ coin.price_change_percentage_1h_in_currency.toFixed(1) }}%
          </td> -->
          <!-- <td
            :class="[
              coin.price_change_percentage_24h_in_currency > 0
                ? 'text-green-500'
                : 'text-red-500',
            ]"
          >
            {{ coin.price_change_percentage_24h_in_currency.toFixed(1) }}%
          </td> -->
          <td
            :class="[
              coin.price_change_percentage_7d_in_currency > 0
                ? 'text-green-500'
                : 'text-red-500',
            ]"
          >
            {{ coin.price_change_percentage_7d_in_currency.toFixed(1) }}%
          </td>
        </tr>
      </tbody>
    </table>
  </main>
</template>
