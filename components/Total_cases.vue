<template>
  <div>
    <div class="latestCheck" v-if="latest == null">
      Houston we have a problem about some components. Try again few minutes later.
    </div>
    <div class="latestWrap" v-if="allCases != null">
      <div class="container dateTime">{{ date }} - {{ time }}</div>
      <div class="container latest">
        <div class="lt">
          <div class="ltLine">
            <span>TOTAL CASES</span>
            <span>
              <money v-model="totalCases" v-bind="money" readonly></money>
            </span>
          </div>
          <div class="ltLine">
            <span>NEW CASES</span>
            <span>
              <money v-model="newCases" v-bind="money" readonly></money>
            </span>
          </div>
        </div>
        <div class="lt">
          <div class="ltLine">
            <span>TOTAL DEATHS</span>
            <span>
              <money v-model="totalDeaths" v-bind="money" readonly></money>
            </span>
          </div>
          <div class="ltLine">
            <span>NEW DEATHS</span>
            <span>
              <money v-model="newDeaths" v-bind="money" readonly></money>
            </span>
          </div>
        </div>
        <div class="lt">
          <div class="ltLine">
            <span>TOTAL RECOVERED</span>
            <span>
              <money v-model="totalRecovered" v-bind="money" readonly></money>
            </span>
          </div>
          <div class="ltLine">
            <span>ACTIVE CASES</span>
            <span>
              <money v-model="activeCases" v-bind="money" readonly></money>
            </span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      latest: '',
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
        suffix: '',
        precision: 0,
        masked: true
      },
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
