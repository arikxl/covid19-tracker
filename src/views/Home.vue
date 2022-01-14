<template>
  <div class="home">Good Luck</div>
</template>

<script>
export default {
  name: "Home",
  components: {},
  methods: {
    async fetchCovidData() {
      const res = await fetch('')
    }
  },
  created() {
    console.log('123')
  },
};
</script>
