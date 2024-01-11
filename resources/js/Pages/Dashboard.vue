<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue'
import { Head, Link } from '@inertiajs/vue3'
import { ref } from 'vue'
import { onMounted } from 'vue'

const lastUpdated = new Date().toLocaleString()

const planes = ref([])

const fetchFlights = async () => {
   const response = await fetch(
      'http://api.aviationstack.com/v1/flights?access_key=83133bf365d382fd5ed7e21d3e3af60f&limit=100&flight_status=active',
   )
   const { data } = await response.json()

   const filteredPlanes = [...data].filter(
      ({ aircraft, live }) => aircraft && live,
   )

   planes.value = filteredPlanes
   console.log(filteredPlanes)
}

onMounted(() => {
   fetchFlights()
})
</script>

<template>
   <Head title="Dashboard" />

   <AuthenticatedLayout>
      <div
         class="grid gap-4 grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 2xl:grid-cols-5"
      >
         <div
            v-for="plane in planes"
            :key="plane.aircraft.registration"
            className="card w-80 m-4 bg-base-100 shadow-xl"
         >
            <figure>
               <img
                  src="https://cdn.jetphotos.com/full/6/1064350_1700504515.jpg"
                  alt=""
               />
            </figure>
            <div className="card-body">
               <h2 className="card-title">
                  {{ plane.aircraft.registration }} | Flight:
                  {{ plane.flight.number }}
               </h2>
               <p class="text-sm">Last Updated: {{ lastUpdated }}</p>
               <div className="card-actions justify-end">
                  <Link
                     :href="route('plane.view', 1)"
                     :data="{ plane }"
                     className="btn btn-primary btn-wide mx-auto mt-4"
                  >
                     Visit
                  </Link>
               </div>
            </div>
         </div>
      </div>
   </AuthenticatedLayout>
</template>
