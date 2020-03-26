<template>
  <div class="containerAll">
    <header>
      <div class="container header"><h1>COVID-19 CORONAVIRUS PANDEMIC | LATEST NEWS FROM THE WORLD</h1></div>
    </header>
    <div class="latestCheck" v-if="latest == null || countries == null">
      Houston we have a problem about some components. Try again few minutes later.
    </div>
    <div class="latestWrap" v-if="latest != null">
      <div class="container latest">
        <div class="lt">
          <span><h2>CORONAVIRUS CASES</h2></span>
          <span>{{ latest.confirmed }}</span>
        </div>
        <div class="lt">
          <span>DEATHS</span>
          <span>{{ latest.deaths }}</span>
        </div>
      </div>
    </div>
    <div class="tableWrap" v-if="countries != null">
      <div class="container table">
        <input type="text" placeholder="Search Country" v-model="search" class="search" />
        <ul>
          <li class="tHead">
            <div>Country</div>
            <div>Total Cases</div>
            <div>Total Deaths</div>
            <div>Total Recovered</div>
          </li>
          <li v-for="country in filteredTable" :key="country.country">
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
