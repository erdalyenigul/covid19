<template>
  <div class="containerAll">
    <header>
      <div class="container header">
        <h1>
          <span>TRACK COVID-19 CORONAVIRUS PANDEMIC</span>
          <span>LATEST NEWS FROM THE WORLD</span>
        </h1>
      </div>
    </header>
    <div class="latestCheck" v-if="latest == null || countries == null">
      Houston we have a problem about some components. Try again few minutes later.
    </div>
    <div class="latestWrap" v-if="latest != null">
      <div class="container dateTime">{{ date }} - {{ time }}</div>
      <div class="container latest">
        <div class="lt">
          <div class="ltLine">
            <span>
              TOTAL CASES
            </span>
            <span>
              {{ totalCases }}
            </span>
          </div>
          <div class="ltLine">
            <span>
              NEW CASES
            </span>
            <span>
              {{ newCases }}
            </span>
          </div>
        </div>
        <div class="lt">
          <div class="ltLine">
            <span>
              TOTAL DEATHS
            </span>
            <span>
              {{ totalDeaths }}
            </span>
          </div>
          <div class="ltLine">
            <span>
              <h2>NEW DEATHS</h2>
            </span>
            <span>
              {{ newDeaths }}
            </span>
          </div>
        </div>
        <div class="lt">
          <div class="ltLine">
            <span>
              TOTAL RECOVERED
            </span>
            <span>
              {{ totalRecovered }}
            </span>
          </div>
          <div class="ltLine">
            <span>
              <h2>ACTIVE CASES</h2>
            </span>
            <span>
              {{ activeCases }}
            </span>
          </div>
        </div>
      </div>
    </div>
    <div class="tableWrap" v-if="countries != null">
      <div class="container table">
        <div class="searchWrap">
          <input type="text" placeholder="Search Country" v-model="search" class="search" />
          <a href="#" class="clearSearch" @click="clearSearch()"><font-awesome-icon prefix="fas" icon="times" /></a>
        </div>
        <ul>
          <li class="tHead">
            <div>ID</div>
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
      justCountries: [],
      totalCases: '',
      newCases: '',
      totalDeaths: '',
      newDeaths: '',
      totalRecovered: '',
      activeCases: '',
      search: '',
      date: '',
      time: ''
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
  methods: {
    clearSearch() {
      this.search = '';
    }
  },
  mounted() {
    this.countries.splice(0, 7);
    this.countries.splice(212, 8);
    console.log(this.countries)

    this.totalCases = this.countries[0].totalCases;
    this.newCases = this.countries[0].newCases;

    this.totalDeaths = this.countries[0].totalDeaths;
    this.newDeaths = this.countries[0].newDeaths;

    this.totalRecovered = this.countries[0].totalRecovered;
    this.activeCases = this.countries[0].activeCases;

    this.date = new Date().toJSON().slice(0, 10).replace(/-/g, '/');
    this.time = new Date().toString().slice(16, 21);
  }
}

</script>
