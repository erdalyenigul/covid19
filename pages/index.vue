<template>
  <div class="containerAll">
    <header>
      <div class="container header">
        <span>COVID-19 CORONAVIRUS PANDEMIC</span>
        <span>LATEST NEWS FROM THE WORLD</span>
      </div>
    </header>
    <div class="latestCheck" v-if="latest == null || countries == null">
      Houston we have a problem about some components. Try again few minutes later.
    </div>
    <div class="latestWrap" v-if="latest != null">
      <div class="container latest">
        <div class="lt">
          <span><h2>CORONAVIRUS CASES</h2></span>
          <span><money v-model="latest.confirmed" v-bind="money" readonly></money></span>
        </div>
        <div class="lt">
          <span>DEATHS</span>
          <span><money v-model="latest.deaths" v-bind="money" readonly></money></span>
        </div>
      </div>
    </div>
    <div class="tableWrap" v-if="countries != null">
      <div class="container table">
        <input type="text" placeholder="Search Country" v-model="search" class="search" />
        <ul>
          <li class="tHead">
            <div>ID</div>
            <div>Country</div>
            <div>Total Cases</div>
            <div>Total Deaths</div>
            <div>Total Recovered</div>
          </li>
          <li v-for="(country, index) in filteredTable" :key="country.country">
            <div>{{ index+1 }}</div>
            <div>{{ country.country }}</div>
            <div>{{ country.totalCases }}</div>
            <div>{{ country.totalDeaths }}</div>
            <div>{{ country.totalRecovered }}</div>
          </li>
        </ul>
      </div>
    </div>
    <footer>
      <div class="container footer">
        <span>Erdal Yenigül, Development by NuxtJS, <a href="mailto:erdalyenigul@gmail.com">erdalyenigul@gmail.com</a></span>
      </div>
    </footer>
  </div>
</template>
<script>

export default {
  data() {
    return {
      latest: '',
      countries: '',
      search: '',
      money: {
        decimal: ',',
        thousands: ',',
        prefix: '',
        suffix: ' ',
        precision: 0,
        masked: false
      }
    }
  },
  async asyncData({ $axios }) {
    let [countries, latest] = await Promise.all([ 
      $axios({
        method: 'get',
        url: 'https://api.collectapi.com/corona/countriesData',
        headers: {
          "content-type": "application/json",
          "authorization": "apikey 62Nz3HHBffJoueZMgYGrzg:6kwzSDcb836gKL7PE8VK6w"
        }
      }),
      $axios({
        method: 'get',
        url: 'https://coronavirus-tracker-api.herokuapp.com/v2/locations'
      }),
    ])
    return {
      countries: countries.data.result,
      latest: latest.data.latest
    }
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
<style>
</style>
