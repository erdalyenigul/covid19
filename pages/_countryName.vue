<template>
  <div class="countryDetailWrap">
    <div class="cdContent">
      <div class="flag"><img :src="flag"></div>
      <div class="countryName">
      	<span class="name">{{ countryDetail.country }}</span>
    		<span class="continent">{{ countryDetail.continent }}</span>
    	</div>
      <ul class="ccDetailBox">

        <li>
	        <span>Today cases</span>
	        <span><input v-model="todayCases" v-money="money" /></span>
        </li>
        <li>
	        <span>Total cases</span>
	        <span><input v-model="cases" v-money="money" /></span>
        </li>
        <li class="perMillion">
	        <span>Cases per one million</span>
	        <span><input v-model="casesPerOneMillion" v-money="money" /></span>
        </li>

        <li>
	        <span>Today deaths</span>
	        <span><input v-model="todayDeaths" v-money="money" /></span>
        </li>
        <li>
	        <span>Total deaths</span>
	        <span><input v-model="deaths" v-money="money" /></span>
        </li>
        <li class="perMillion">
	        <span>Deaths per one million</span>
	        <span><input v-model="deathsPerOneMillion" v-money="money" /></span>
        </li>

        <li>
	        <span>Total recovered</span>
	        <span><input v-model="recovered" v-money="money" /></span>
        </li>
        <li class="perMillion">
	        <span>Recovered per one million</span>
	        <span><input v-model="recoveredPerOneMillion" v-money="money" /></span>
        </li>

        <li>
	        <span>Active</span>
	        <span><input v-model="active" v-money="money" /></span>
        </li>
        <li class="perMillion">
	        <span>Active per one million</span>
	        <span><input v-model="activePerOneMillion" v-money="money" /></span>
        </li>

        <li>
	        <span>Tests</span>
	        <span><input v-model="tests" v-money="money" /></span>
        </li>
        <li class="perMillion">
	        <span>Test per one million</span>
	        <span><input v-model="testsPerOneMillion" v-money="money" /></span>
        </li>

        <li>
	        <span>Critical</span>
	        <span><input v-model="critical" v-money="money" /></span>
        </li>
        <li class="perMillion">
	        <span>Critical per one million</span>
	        <span><input v-model="criticalPerOneMillion" v-money="money" /></span>
        </li>

        <li>
	        <span>Population</span>
	        <span><input v-model="population" v-money="money" /></span>
        </li>
      </ul>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      countryDetail: '',
      flag: '',

      todayCases: '',
      cases: '',
      casesPerOneMillion : '',

      todayDeaths : '',
      deaths : '',
      deathsPerOneMillion : '',

      recovered : '',
      recoveredPerOneMillion : '',

      active : '',
      activePerOneMillion : '',

      tests : '',
      testsPerOneMillion : '',

      critical : '',
      criticalPerOneMillion : '',

      population : '',


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
  mounted() {
    for (var i = 0; i < this.countries.length; i++) {
      if (this.$route.params.countryName == this.countries[i].country) {
        console.log(this.countries[i]);
        this.countryDetail = this.countries[i];
        this.flag = this.countries[i].countryInfo.flag;

        this.todayCases = this.countries[i].todayCases;
        this.cases = this.countries[i].cases;
        this.casesPerOneMillion  = this.countries[i].casesPerOneMillion ;

        this.todayDeaths  = this.countries[i].todayDeaths ;
        this.deaths  = this.countries[i].deaths ;
        this.deathsPerOneMillion  = this.countries[i].deathsPerOneMillion ;

        this.recovered  = this.countries[i].recovered ;
        this.recoveredPerOneMillion  = this.countries[i].recoveredPerOneMillion ;

        this.active  = this.countries[i].active ;
        this.activePerOneMillion  = this.countries[i].activePerOneMillion ;

        this.tests  = this.countries[i].tests ;
        this.testsPerOneMillion  = this.countries[i].testsPerOneMillion ;

        this.critical  = this.countries[i].critical ;
        this.criticalPerOneMillion  = this.countries[i].criticalPerOneMillion ;
        
        this.population  = this.countries[i].population ;
      }
    }
  }
}

</script>
