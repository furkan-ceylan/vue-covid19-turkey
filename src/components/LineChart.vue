

<script>
import { Line } from "vue-chartjs";

export default {
  extends: Line,
  props: {
    label: {
      type: String,
    },
    chartdata: {
      type: String,
      default: null,
    },
    options: {
      type: Object,
      default: null,
    },
  },
  mounted() {  
    const date = this.chartData.map(d => data[date]).reverse();
    const totals = this.chartData.map(d => data[date].d).reverse();

    this.renderChart({
        labels: dates,
        datasets: [{ 
            label: this.label,
            data: totals
        }],
    },
    this.options);
  },
};
</script>
