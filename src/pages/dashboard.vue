<template>
  <q-page>
    <div class="row q-col-gutter-sm q-ma-xs">
      <div class="col-4">
        <q-card>
          <q-card-section class="bg-blue-8 text-white">
            <div class="row">
              <div class="col-10">
                <div class="text-h6">Sales</div>
                <div class="text-h5">160</div>
              </div>
              <div class="col-2">
                <q-icon size="70px" name="trending_up"/>
              </div>
            </div>
          </q-card-section>
        </q-card>
      </div>
      <div class="col-4">
        <q-card>
          <q-card-section class="bg-green-8 text-white">
            <div class="row">
              <div class="col-10">
                <div class="text-h6">Goals</div>
                <div class="text-h5">140</div>
              </div>
              <div class="col-2">
                <q-icon size="70px" name="far fa-dot-circle"/>
              </div>
            </div>
          </q-card-section>
        </q-card>
      </div>
      <div class="col-4">
        <q-card>
          <q-card-section class="bg-orange-9 text-white">
            <div class="row">
              <div class="col-10">
                <div class="text-h6">% Change</div>
                <div class="text-h5">
                  <q-icon name="arrow_downward"/>
                  2%
                </div>
              </div>
              <div class="col-2">
                <q-icon size="70px" name="compare_arrows"/>
              </div>
            </div>
          </q-card-section>
        </q-card>
      </div>
    </div>
    <div>
      <draggable class="row q-col-gutter-sm q-ma-xs" group="people" @start="drag=true" @end="drag=false">
        <div class="col-4">
          <q-card flat bordered class="">
            <q-card-section>
              <div class="text-h6">Sales vs Goals</div>
            </q-card-section>

            <q-separator inset></q-separator>

            <q-card-section>
              <IEcharts :option="barChartOption" :resizable="true" style="height:220px"/>
            </q-card-section>
          </q-card>
        </div>
        <div class="col-4">

          <q-card flat bordered class="">
            <q-card-section>
              <div class="text-h6">Market Share & Growth</div>
            </q-card-section>

            <q-separator inset></q-separator>

            <q-card-section>
              <IEcharts :option="lineChartOption" :resizable="true" style="height:220px"/>
            </q-card-section>
          </q-card>
        </div>
        <div class="col-4">

          <q-card flat bordered class="">
            <q-card-section>
              <div class="text-h6">Sales vs. Quota</div>
            </q-card-section>

            <q-separator inset></q-separator>

            <q-card-section>
              <IEcharts :option="stackedBarOptions" :resizable="true" style="height:220px"/>
            </q-card-section>
          </q-card>
        </div>
      </draggable>
    </div>
  </q-page>
</template>

<script>
    import Vue from 'vue';
    import IEcharts from 'vue-echarts-v3/src/full.js';
    import draggable from 'vuedraggable'

    Vue.component('IEcharts', IEcharts);
    Vue.component('draggable', draggable);

    export default {
        name: "dashboard",
        data() {
            return {
                barChartOption: {
                    grid: {
                        bottom: '20%',
                        left: '15%',
                        top: '3%'
                    },
                    legend: {
                        bottom: 0
                    },
                    toolbox: {
                        feature: {
                            saveAsImage: {title: "Download"}
                        }
                    },
                    tooltip: {},
                    dataset: {
                        dimensions: ['time_period', 'sale', 'goal'],
                        source: [
                            {time_period: 'Jan 2019', sale: 50, goal: 70},
                            {time_period: 'Feb 2019', sale: 80, goal: 75},
                            {time_period: 'Mar 2019', sale: 86, goal: 80},
                            {time_period: 'Apr 2019', sale: 72, goal: 85}
                        ]
                    },
                    xAxis: {
                        type: 'category',
                        // axisLabel: {
                        //     rotate: 45
                        // }
                    },
                    yAxis: {
                        // name: 'Goal',
                        // nameLocation: 'center',
                        // nameGap: 30,
                        // nameTextStyle:{
                        //     fontWeight: 'bold'
                        // }
                    },
                    series: [
                        {type: 'bar', name: 'Sales'},
                        {type: 'bar', name: 'Goals'}
                    ]
                },
                lineChartOption: {
                    grid: {
                        bottom: '20%',
                        left: '15%',
                        top: '3%'
                    },
                    legend: {
                        bottom: 0
                    },
                    toolbox: {
                        feature: {
                            saveAsImage: {title: "Download"}
                        }
                    },
                    tooltip: {
                        // formatter:
                        //     function (param) {
                        //     console.log(param)
                        //     // return param.seriesName + '<br />' + param.name + ': ';
                        // }
                    },
                    dataset: {
                        dimensions: ['product_name', 'share', 'growth'],
                        source: [
                            {product_name: 'Product A', share: 20, growth: 25},
                            {product_name: 'Product B', share: 22, growth: 18},
                            {product_name: 'Product C', share: 40, growth: 45}
                        ]
                    },
                    xAxis: {
                        type: 'category',
                        // axisLabel: {
                        //     rotate: 45
                        // }
                    },
                    yAxis: {
                        axisLabel: {
                            formatter: function (value, index) {
                                return value + ' %'
                            }
                        }
                        // name: 'Goal',
                        // nameLocation: 'center',
                        // nameGap: 30,
                        // nameTextStyle:{
                        //     fontWeight: 'bold'
                        // }
                    },
                    series: [
                        {type: 'line', name: 'Share'},
                        {type: 'line', name: 'Growth'}
                    ]
                },
                gaugeOptions: {
                    tooltip: {
                        formatter: '{a} <br/>{b} : {c}%'
                    },
                    toolbox: {
                        feature: {
                            saveAsImage: {title: "Download"}
                        }
                    },
                    series: [
                        {
                            type: 'gauge',
                            detail: {formatter: '{value}%'},
                            data: [{value: 30}]
                        }
                    ]
                },
                stackedBarOptions: {
                    tooltip: {
                        trigger: 'axis',
                        axisPointer: {
                            type: 'shadow'
                        },
                        backgroundColor: 'rgba(50,50,50,0.9)',

                    },
                    color: ['#3395dd', '#ed7d31', '#3b3b3b'],
                    // legend: {
                    //     y: "bottom",
                    //     data: [{name: 'Territory Sales', icon: 'circle'}, {
                    //         name: 'Remaining Nation Sales',
                    //         icon: 'circle'
                    //     }]
                    // },
                    grid: {
                        x: 70,
                        y: 30,
                        x2: 45,
                        y2: 50
                    },
                    toolbox: {
                        show: false
                    },
                    calculable: true,
                    xAxis:
                        {
                            type: 'value',
                            position: 'top',
                            axisLine: {
                                show: false
                            }
                        },
                    yAxis: [
                        {
                            type: 'category',
                            data: ['Alex Morrow', 'Joanna Carter', 'Jimmy Joanna', 'Mack Hales'],
                            axisLabel: {fontSize: 9}
                        }
                    ],
                    series: [{
                        name: 'Qualification',
                        type: 'bar',
                        stack: 'A',
                        data: [300, 350, 400, 500]

                    },{
                        name: 'Discovery',
                        type: 'bar',
                        stack: 'A',
                        data: [100, 180, 250, 300]

                    },{
                        name: 'Quote',
                        type: 'bar',
                        stack: 'A',
                        data: [100, 120, 200, 220]

                    }]
                }
            }
        }
    }
</script>

<style scoped>

</style>
