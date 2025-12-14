<script setup>
import { ref, computed } from "vue";
import { useCoinsStore } from "../store/coinsStore";

const coinStore = useCoinsStore();

const sortBy = ref(null);

const sortedCoins = computed(() => {
  const coins = [...coinStore.coinDataTop50];

  if (sortBy.value === "change") {
    return coins.sort(
        (a, b) =>
            parseFloat(b.changePercent24Hr) -
            parseFloat(a.changePercent24Hr)
    );
  }

  if (sortBy.value === "volume") {
    return coins.sort(
        (a, b) =>
            parseFloat(b.volumeUsd24Hr) -
            parseFloat(a.volumeUsd24Hr)
    );
  }

  return coins;
});
</script>

<template>
  <div class="bg-[#1B2028] p-6 rounded-lg text-white">
    <h2 class="text-2xl font-bold mb-4 text-center">
      Live Market
    </h2>

    <div class="flex justify-center gap-4 mb-6">
      <button
          @click="sortBy = 'change'"
          class="px-4 py-2 bg-blue-600 rounded hover:bg-blue-700 transition"
      >
        Sort by Change %
      </button>

      <button
          @click="sortBy = 'volume'"
          class="px-4 py-2 bg-green-600 rounded hover:bg-green-700 transition"
      >
        Sort by Volume 24h
      </button>
    </div>

    <table class="w-full text-sm text-left">
      <thead class="border-b border-gray-600">
      <tr>
        <th class="py-2">Name</th>
        <th class="py-2">Price</th>
        <th class="py-2">Change 24h</th>
        <th class="py-2">Volume 24h</th>
      </tr>
      </thead>

      <tbody>
      <tr
          v-for="coin in sortedCoins"
          :key="coin.id"
          class="border-b border-gray-700 hover:bg-[#232a35]"
      >
        <td class="py-2 font-semibold">
          {{ coin.name }} ({{ coin.symbol }})
        </td>

        <td class="py-2">
          ${{ parseFloat(coin.priceUsd).toFixed(2) }}
        </td>

        <td
            class="py-2"
            :class="{
              'text-green-400': parseFloat(coin.changePercent24Hr) > 0,
              'text-red-400': parseFloat(coin.changePercent24Hr) < 0
            }"
        >
          {{ parseFloat(coin.changePercent24Hr).toFixed(2) }}%
        </td>

        <td class="py-2">
          ${{ Number(coin.volumeUsd24Hr).toLocaleString() }}
        </td>
      </tr>
      </tbody>
    </table>
  </div>
</template>
