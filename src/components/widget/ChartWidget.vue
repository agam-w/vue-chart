<template>
    <highcharts style="width:100%" :options="chartOptions"></highcharts>
</template>
<script>
import {Chart} from 'highcharts-vue'
export default {
    props: {
        data: {
            required: true,
            type: Array
        },
        categories: {
            required: true,
            type: Array
        },
    },
    components: {
        highcharts: Chart ,
    },
    data() {
        return {
            chartOptions: {
                chart: {
                    type: 'spline',
                },
                title: false,
                subtitle: false,
                xAxis: {
                    categories: [],
                    tickInterval: 25,
                },
                yAxis: {
                    
                },
                tooltip: {
                    crosshairs: true,
                    shared: true
                },
                plotOptions: {
                    spline: {
                        marker: {
                            radius: 4,
                            lineColor: '#666666',
                            lineWidth: 1
                        }
                    }
                },
                series: {
                    name: 'Tokyo',
                    marker: {
                    symbol: 'square'
                    },
                    data: this.data
                },
                credits:{
                    enabled:false,
                }
            }
        }
    },
    watch: {
        data (newVal) {
            this.chartOptions.series.data = newVal
        },
        categories (newVal) {
            this.chartOptions.xAxis.categories = newVal
        }
    },
}
</script>
<style lang="scss" scoped>
    @import './chart.scss';
</style>