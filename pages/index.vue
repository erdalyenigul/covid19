<template>
  <div class="containerAll">
    <header>
      <div class="container header">
        <span>
          <h1>TRACK COVID-19 CORONAVIRUS PANDEMIC</h1>
        </span>
        <span>
          <h2>LATEST NEWS FROM THE WORLD</h2>
        </span>
      </div>
    </header>
    <div class="latestCheck" v-if="latest == null || countries == null">
      Houston we have a problem about some components. Try again few minutes later.
    </div>
    <div class="latestWrap" v-if="allCases != null">
      <div class="container dateTime">{{ date }} - {{ time }}</div>
      <div class="container latest">
        <div class="lt">
          <div class="ltLine">
            <span>TOTAL CASES</span>
            <span><money v-model="totalCases" v-bind="money" readonly></money></span>
          </div>
          <div class="ltLine">
            <span>NEW CASES</span>
            <span><money v-model="newCases" v-bind="money" readonly></money></span>
          </div>
        </div>
        <div class="lt">
          <div class="ltLine">
            <span>TOTAL DEATHS</span>
            <span><money v-model="totalDeaths" v-bind="money" readonly></money></span>
          </div>
          <div class="ltLine">
            <span>NEW DEATHS</span>
            <span><money v-model="newDeaths" v-bind="money" readonly></money></span>
          </div>
        </div>
        <div class="lt">
          <div class="ltLine">
            <span>TOTAL RECOVERED</span>
            <span><money v-model="totalRecovered" v-bind="money" readonly></money></span>
          </div>
          <div class="ltLine">
            <span>ACTIVE CASES</span>
            <span><money v-model="activeCases" v-bind="money" readonly></money></span>
          </div>
        </div>
      </div>
    </div>
    <div class="tableWrap" v-if="countries != null">
      <div class="container table">
        <div class="searchWrap">
          <input type="text" placeholder="Search Country" v-model="search" class="search" />
        </div>
        <ul>
          <li class="tHead">
            <div></div>
            <div>Country</div>
            <div>Total Cases</div>
            <div>Total Deaths</div>
            <div>Total Recovered</div>
          </li>
          <li v-for="(country, index) in filteredTable" :key="index">
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
      countries: [],
      allCases: [],
      totalCases: '',
      newCases: '',
      totalDeaths: '',
      newDeaths: '',
      totalRecovered: '',
      activeCases: '',
      search: '',
      date: '',
      time: '',
      money: {
        decimal: ',',
        thousands: ',',
        prefix: '',
        suffix: ' ',
        precision: 0,
        masked: false
      },
    }
  },
  asyncData({ $axios, error }) {
    return $axios({
        method: 'get',
        url: 'https://api.collectapi.com/corona/countriesData',
        headers: {
          "content-type": "application/json",
          "authorization": "apikey 62Nz3HHBffJoueZMgYGrzg:6kwzSDcb836gKL7PE8VK6w"
        }
      })
      .then((response) => {
        return { countries: response.data.result }
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
  },
  mounted() {
    this.date = new Date().toJSON().slice(0, 10).replace(/-/g, '/');
    this.time = new Date().toString().slice(16, 21);

    this.$axios({
      method: 'get',
      url: 'https://corona.lmao.ninja/v2/all',
      headers: {
        "content-type": "application/json",
        "authorization": "apikey 62Nz3HHBffJoueZMgYGrzg:6kwzSDcb836gKL7PE8VK6w"
      }
    })
    .then((response) => {
      console.log(response);
      this.allCases = response.data;

      this.totalCases = this.allCases.cases;
      this.newCases = this.allCases.todayCases;

      this.totalDeaths = this.allCases.deaths;
      this.newDeaths = this.allCases.todayDeaths;

      this.totalRecovered = this.allCases.recovered;
      this.activeCases = this.allCases.active;
    })
    .catch((error) => {
      console.log(error);
    })

  }
}

</script>
