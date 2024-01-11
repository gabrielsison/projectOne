<script setup>
import Chart from '@/Components/Chart.vue'

const charts = [
   {
      id: 'socChart',
      name: 'State of Charge',
   },
   {
      id: 'vsChart',
      name: 'Vertical Speed',
   },
   {
      id: 'acclyChart',
      name: 'Acceleration Y',
   },
   {
      id: 'vibaChart',
      name: 'Vibration A',
   },
   {
      id: 'vibbChart',
      name: 'Vibration B',
   },
]
</script>

<template>
   <section class="w-full">
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 p-4">
         <div
            v-for="{ id, name } in charts"
            :key="id"
            className="card shadow-lg p-2 bg-slate-50"
         >
            <figure>
               <Chart :id="id" :name="name" />
            </figure>
         </div>
      </div>
   </section>
</template>
