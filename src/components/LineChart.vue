<script>
import VueCharts from 'vue-chartjs'
//import { Line, mixins } from 'vue-chartjs'

export default {
    extends: VueCharts.Line,
    //mixins: [mixins.reactiveProp],
    props:{
        label:{
            type:String
        },
        chartData:{
            type:Array
        },
        options:{
            type:Object
        }
    },
    mounted() {
        // const dates = this.chartData.map(d => d.date).reverse()
        // const totals = this.chartData.map(d => d.total).reverse()
        console.log(this.chartData)
        this.renderChart({
            labels: this.chartData.map(d => d.date).reverse(),
            datasets:[{
                    label: this.label,
                    data: this.chartData.map(d => d.total).reverse()
                }],
        }, this.options)
    },
}
</script>
