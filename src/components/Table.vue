<template>
  <div class="row">
    <!-- <LineChart :chartdata="dataTable" :options="chartOptions" label="Cases"/> -->
    <div class="col-sm-12 col-md-12 col-xl-12">
      <div class="main-card mb-3 card">
        <div class="card-body">
          <table
            style="width: 100%;"
            id="example"
            class="table table-hover table-striped table-bordered"
          >
            <thead>
              <tr>
                <th>Date</th>
                <th>Tests</th>
                <th>Cases</th>
                <th>Deaths</th>
                <th>Recovered</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="data in dataTable" :key="data.id">
                <td>{{ data.date }}</td>
                <td>{{ numberWithCommas(data.tests) }}</td>
                <td>{{ data.cases }}</td>
                <td>{{ data.deaths }}</td>
                <td>{{ data.recovered }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import moment from "moment";
import LineChart from "@/components/LineChart.vue";

export default {
  name: "Table",
  components: {
    LineChart,
  },
  data() {
    return {
      dataTable: ["date", "tests", "cases", "deaths", "recovered"],

      chartOptions: {
        responsive: true,
        maintainAspectRatio: false,
      },
    };
  },

  methods: {
    async fetchAllData() {
      const res = await fetch(
        "https://raw.githubusercontent.com/ozanerturk/covid19-turkey-api/master/dataset/timeline.json"
      );
      this.dataTable = await res.json();
      return this.dataTable;
    },
    numberWithCommas(x) {
      return x.replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    },
  },
  async created() {
    const data = await this.fetchAllData();
    const date = moment().format("YYYY/MM/DD") - 1;
    // console.log(data)
  },
};
</script>

<style></style>
