<template>
  <div class="app-main__outer">
    <div class="app-main__inner">
      <div class="app-page-title">
        <div class="page-title-wrapper">
          <div class="page-title-heading">
            COVID19 TURKEY DAILY CASES
          </div>
          <div class="page-title-heading2">
            {{ title }}
          </div>
        </div>
      </div>
      <div class="tabs-animation">
        <div class="row">
          <div class="col-sm-12 col-md-6 col-xl-3">
            <div
              class="card mb-3 bg-primary widget-chart text-white card-border"
            >
              <div class="icon-wrapper rounded-circle">
                <div class="icon-wrapper-bg bg-white opacity-1"></div>
                <i class="fas fa-vials text-white"></i>
              </div>
              <div class="widget-numbers">{{ numberWithCommas(retTests) }}</div>
              <div
                class="widget-subheading fsize-2 pt-2 opacity-10 font-weight-bold"
              >
                Today's Tests
              </div>
            </div>
          </div>
          <div class="col-sm-12 col-md-6 col-xl-3">
            <div
              class="card mb-3 bg-warning widget-chart text-white card-border"
            >
              <div class="icon-wrapper rounded-circle">
                <div class="icon-wrapper-bg bg-white opacity-2"></div>
                <i class="fas fa-virus text-white"></i>
              </div>
              <div class="widget-numbers">{{ numberWithCommas(retCases) }}</div>
              <div
                class="widget-subheading fsize-2 pt-2 opacity-10 font-weight-bold"
              >
                Cases
              </div>
            </div>
          </div>
          <div class="col-sm-12 col-md-6 col-xl-3">
            <div
              class="card mb-3 bg-danger widget-chart text-white card-border"
            >
              <div class="icon-wrapper rounded">
                <div class="icon-wrapper-bg bg-white opacity-2"></div>
                <i class="fas fa-skull-crossbones text-white"></i>
              </div>
              <div class="widget-numbers">
                {{ numberWithCommas(retDeaths) }}
              </div>
              <div
                class="widget-subheading fsize-2 pt-2 opacity-10 font-weight-bold"
              >
                Deaths
              </div>
            </div>
          </div>
          <div class="col-sm-12 col-md-6 col-xl-3">
            <div
              class="card mb-3 bg-success widget-chart text-white card-border"
            >
              <div class="icon-wrapper rounded">
                <div class="icon-wrapper-bg bg-white opacity-5"></div>
                <i class="fas fa-plus-square text-white"></i>
              </div>
              <div class="widget-numbers">
                {{ numberWithCommas(retRecovered) }}
              </div>
              <div
                class="widget-subheading fsize-2 pt-2 opacity-10 font-weight-bold"
              >
                Recovered
              </div>
            </div>
          </div>
        </div>
        <div class="text-center mbg-3"></div>
        <Table />
      </div>
    </div>
  </div>
</template>

<script>
import moment from "moment";
import Table from "@/components/Table.vue";

export default {
  name: "App",
  components: {
    Table,
  },
  data() {
    return {
      retCases: "-",
      retTests: "-",
      retDeaths: "-",
      retRecovered: "-",
      dataDate: [],
      title: "Waiting for today's data...",
    };
  },
  methods: {
    async fetchDailyData() {
      const res = await fetch(
        "https://api.apify.com/v2/key-value-stores/28ljlt47S5XEd1qIi/records/LATEST?disableRedirect=true"
      );
      const data = await res.json();
      return data;
    },
    numberWithCommas(x) {
      return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    },
  },
  async created() {
    const data = await this.fetchDailyData();
    const date = moment(data.lastUpdatedAtSource).format("DD/MM/YYYY");

    this.retCases = data.dailyInfected;
    this.retTests = data.dailyTested;
    this.retDeaths = data.dailyDeceased;
    this.retRecovered = data.dailyRecovered;
    this.title = date;
  },
};
</script>
