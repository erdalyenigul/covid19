<template>
  <div>
    <appTotalCases></appTotalCases>
    <div class="tableWrap" v-if="countries != null">
      <div class="container table">
        <div class="searchWrap">
          <input type="text" placeholder="Search Country" v-model="search" class="search" />
        </div>
        <ul>
          <li class="tHead">
            <span></span>
            <span>Country</span>
            <span>Total Cases</span>
            <span>Total Deaths</span>
            <span>Total Recovered</span>
          </li>
          <li v-for="(item, index) in filteredTable" :key="index">
            <nuxt-link :to="{ name: 'countryName', params: { countryName : item.country } }">
              <span><img :src="item.countryInfo.flag"></span>
              <span>{{ item.country }}</span>
              <span><input v-model="item.cases" v-money="money" /></span>
              <span><input v-model="item.deaths" v-money="money" /></span>
              <span><input v-model="item.recovered" v-money="money" /></span>
            </nuxt-link>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>
<script>
import Total_cases from '@/components/Total_cases';

export default {
  data() {
    return {
      countries: [],
      search: '',
      money: {
        decimal: ',',
        thousands: ',',
        prefix: '',
        suffix: '',
        precision: 0,
        masked: true
      },
    }
  },
  components: {
    appTotalCases: Total_cases
  },
  asyncData({ $axios, error }) {
    return $axios({
        method: 'get',
        url: 'https://corona.lmao.ninja/v2/countries?sort=cases',
        headers: {
          "content-type": "application/json"
        }
      })
      .then((response) => {
        return { countries: response.data }
      })
      .catch((error) => {
        console.log(error);
      })
  },
  computed: {
    filteredTable() {
      return this.countries.filter((country) => {
        return country.country.toLowerCase().match(this.search.toLowerCase());
      });
    }
  }
}
</script>
