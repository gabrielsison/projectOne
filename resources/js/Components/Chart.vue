<script setup>
import ApexCharts from 'apexcharts'
import { onMounted } from 'vue'

const { id, name } = defineProps({ id: String, name: String })

var options = {
   series: [
      {
         name: 'Sales',
         data: generateChartData().slice(),
      },
   ],
   chart: {
      height: 350,
      type: 'line',
   },
   forecastDataPoints: {
      count: 7,
   },
   stroke: {
      width: 5,
      curve: 'smooth',
   },
   xaxis: {
      type: 'datetime',
      categories: [
         '1/11/2000',
         '2/11/2000',
         '3/11/2000',
         '4/11/2000',
         '5/11/2000',
         '6/11/2000',
         '7/11/2000',
         '8/11/2000',
         '9/11/2000',
         '10/11/2000',
         '11/11/2000',
         '12/11/2000',
         '1/11/2001',
         '2/11/2001',
         '3/11/2001',
         '4/11/2001',
         '5/11/2001',
         '6/11/2001',
      ],
      tickAmount: 10,
      labels: {
         formatter: function (value, timestamp, opts) {
            return opts.dateFormatter(new Date(timestamp), 'dd MMM')
         },
      },
   },
   title: {
      text: name,
      align: 'left',
      style: {
         fontSize: '16px',
         color: '#666',
      },
   },
   fill: {
      type: 'gradient',
      gradient: {
         shade: 'dark',
         gradientToColors: ['#FDD835'],
         shadeIntensity: 1,
         type: 'horizontal',
         opacityFrom: 1,
         opacityTo: 1,
         stops: [0, 100, 100, 100],
      },
   },
   yaxis: {
      min: -10,
      max: 40,
   },
}

function generateChartData() {
   // Generating sample data, you can replace this with your own data logic
   const numberOfDataPoints = Math.floor(Math.random() * 500)
   const data = []

   for (let i = 1; i <= numberOfDataPoints; i++) {
      data.push({
         x: i,
         y: Math.floor(Math.random() * 100) + 1, // Random Y values for demonstration
      })
   }

   return data
}

onMounted(() => {
   var chart = new ApexCharts(document.querySelector(`#${id}`), options)
   chart.render()
})
</script>

<template>
   <div :id="id" class="w-full"></div>
</template>
