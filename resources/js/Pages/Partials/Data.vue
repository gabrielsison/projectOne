<script setup>
import Chart from '@/Components/Chart.vue'
</script>

<template>
   <section class="w-full md:w-2/3">
      <div class="grid grid-cols-1 xl:grid-cols-2 gap-4 p-4">
         <div v-for="n in 6" :key="n" className="card shadow-lg p-2">
            <figure>
               <Chart :id="`chart${n}`" />
            </figure>
         </div>
      </div>
   </section>
</template>
