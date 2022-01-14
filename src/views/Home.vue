<template>
  <main v-if="!isLoading">
    <DataTitle :text="title" :dataDate="dataDate" />
    <DataBoxes :stats="stats" />
    <CountrySelect @getCountry="getCountryData" :countries="countries" />
    <button
      v-if="stats.Country"
      @click="clearCountryData"
      class="
        bg-green-700
        text-white
        rounded
        p-3
        focus:outline-none
        hover:bg-green-600
        mb-4
      "
    >
      Clear Country
    </button>
  </main>
  <main class="flex flex-col align-center justify-center text-center" v-else>
    <div class="text-grey-500 text-3xl mt-10 mb-6">Loading...</div>
    <img :src="loadingImg" alt="" class="w-24 m-auto" />
  </main>
</template>

<script>
import DataTitle from "../components/DataTitle.vue";
import DataBoxes from "../components/DataBoxes.vue";
import CountrySelect from "../components/CountrySelect.vue";

export default {
  name: "Home",
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect,
  },
  data() {
    return {
      isLoading: true,
      title: "Global",
      dataDate: "",
      stats: {},
      countries: [],
      loadingImg: require("../assets/hourglass.gif"),
    };
  },
  methods: {
    async fetchCovidData() {
      const res = await fetch("https://api.covid19api.com/summary");
      const data = await res.json();
      return data;
    },
    getCountryData(country) {
      this.stats = country;
      this.title = country.Country;
    },
    async clearCountryData() {
      this.isLoading = true;
      const data = await this.fetchCovidData();
      this.title = "Global";
      this.stats = data.Global;
      this.isLoading = false;
    },
  },
  async created() {
    const data = await this.fetchCovidData();
    this.dataDate = data.Date;
    this.stats = data.Global;
    this.countries = data.Countries;
    this.isLoading = false;
  },
};
</script>
