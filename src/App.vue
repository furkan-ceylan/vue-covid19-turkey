<template>
  <div class="app-main__outer">
    <div class="app-main__inner">
      <div class="app-page-title">
        <div class="page-title-wrapper">
          <div class="page-title-heading bold">
            <div>
              COVID19 TURKEY DAILY CASES
              <div class="page-title-heading">
                {{title}}
              </div>
            </div>
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
              <div class="widget-numbers">{{ retTests  | numberFormat}}</div>
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
              <div class="widget-numbers">{{ retCases | numberFormat}}</div>
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
              <div class="widget-numbers">{{ retDeaths | numberFormat}}</div>
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
                <div class="icon-wrapper-bg bg-focus opacity-5"></div>
                <i class="fas fa-plus-square text-white"></i>
              </div>
              <div class="widget-numbers">{{ retRecovered | numberFormat}}</div>
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
      retCases: "Loading...",
      retTests: "Loading...",
      retDeaths: "Loading...",
      retRecovered: "Loading...",
      dataDate: [],
      title: "Today's datas is waiting...",
    };
  },
  setup () {
    return {
      numberWithCommas (x) {
        return x.toString()
          .replace(/\B(?=(\d{3})+(?!\d))/g, ',');
      }
    };
  },
 methods: {
    async fetchDailyData() {
      const res = await fetch(
        "https://raw.githubusercontent.com/ozanerturk/covid19-turkey-api/master/dataset/timeline.json"
      );
      this.dataDate = await res.json();
      return this.dataDate
    }
  },
  async created() {
    const data = await this.fetchDailyData();
    const date = moment().format("DD/MM/YYYY");
    
    const datawithDate = data[date];
    console.log(data);

    this.retCases = datawithDate.cases;
    this.retTests = datawithDate.tests;
    this.retDeaths = datawithDate.deaths;
    this.retRecovered = datawithDate.recovered;
    this.title = datawithDate.date;
    
  },
};
</script>
