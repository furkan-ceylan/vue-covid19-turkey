<template>
  <div class="row">
    <div class="col-sm-12 col-md-12 col-xl-12">
      <div class="mb-3 card">
        <div class="card-header-tab card-header">
          <div>All Cases</div>
          <div class="btn-actions-pane-right">
            <div role="group" class="btn-group-sm btn-group">
              <button class="btn-shadow btn btn-dark">Refresh</button>
            </div>
          </div>
        </div>
        <div class="card-body">
          <table
            style="width: 100%;"
            id="example2"
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
                <td>{{ data.tests }}</td>
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

export default {
  name: "Table",
  data() {
    return {
      dataTable: [],
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
  },
  async created() {
    const data = await this.fetchAllData();
    const date = moment().format("DD/MM/YYYY");

    console.log(data[date].cases);
  },
  setup () {
    return {
      numberWithCommas (x) {
        return x.toString()
          .replace(/\B(?=(\d{3})+(?!\d))/g, ',');
      }
    };
  },
};
</script>

<style></style>
