<template>
  <div class="countryDetailWrap">
    <div class="cdContent">
      <div class="flag">
        <img :src="countryDetail.countryInfo.flag" />
      </div>
      <div class="countryName">
        <span class="name">{{ countryDetail.country }}</span>
        <span class="continent">{{ countryDetail.continent }}</span>
      </div>
      <ul class="ccDetailBox">
        <li>
          <span>Today cases</span>
          <span>
            <input v-model="countryDetail.todayCases" v-money="money" />
          </span>
        </li>
        <li>
          <span>Total cases</span>
          <span>
            <input v-model="countryDetail.cases" v-money="money" />
          </span>
        </li>
        <li class="perMillion">
          <span>Cases per one million</span>
          <span>
            <input v-model="countryDetail.casesPerOneMillion" v-money="money" />
          </span>
        </li>

        <li>
          <span>Today deaths</span>
          <span>
            <input v-model="countryDetail.todayDeaths" v-money="money" />
          </span>
        </li>
        <li>
          <span>Total deaths</span>
          <span>
            <input v-model="countryDetail.deaths" v-money="money" />
          </span>
        </li>
        <li class="perMillion">
          <span>Deaths per one million</span>
          <span>
            <input v-model="countryDetail.deathsPerOneMillion" v-money="money" />
          </span>
        </li>

        <li>
          <span>Total recovered</span>
          <span>
            <input v-model="countryDetail.recovered" v-money="money" />
          </span>
        </li>
        <li class="perMillion">
          <span>Recovered per one million</span>
          <span>
            <input v-model="countryDetail.recoveredPerOneMillion" v-money="money" />
          </span>
        </li>

        <li>
          <span>Active</span>
          <span>
            <input v-model="countryDetail.active" v-money="money" />
          </span>
        </li>
        <li class="perMillion">
          <span>Active per one million</span>
          <span>
            <input v-model="countryDetail.activePerOneMillion" v-money="money" />
          </span>
        </li>

        <li>
          <span>Tests</span>
          <span>
            <input v-model="tests" v-money="money" />
          </span>
        </li>
        <li class="perMillion">
          <span>Test per one million</span>
          <span>
            <input v-model="countryDetail.testsPerOneMillion" v-money="money" />
          </span>
        </li>

        <li>
          <span>Critical</span>
          <span>
            <input v-model="countryDetail.critical" v-money="money" />
          </span>
        </li>
        <li class="perMillion">
          <span>Critical per one million</span>
          <span>
            <input v-model="countryDetail.criticalPerOneMillion" v-money="money" />
          </span>
        </li>

        <li>
          <span>Population</span>
          <span>
            <input v-model="countryDetail.population" v-money="money" />
          </span>
        </li>
      </ul>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      countryDetail: "",
      money: {
        decimal: ",",
        thousands: ",",
        prefix: "",
        suffix: "",
        precision: 0,
        masked: true
      }
    };
  },
  created() {
    this.countryDetail = this.$route.params.countryDetail;
  }
};
</script>
