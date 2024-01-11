<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue'
import { Head, Link } from '@inertiajs/vue3'
import Map from '@/Components/Map.vue'
import Data from '@/Pages/Partials/Data.vue'

const { plane } = defineProps({ plane: Object })
const { aircraft, airline, live, flight } = plane
</script>

<template>
   <Head title="Dashboard" />

   <AuthenticatedLayout>
      <div class="flex flex-col-reverse grow">
         <section class="w-full max-w-7xl mx-auto">
            <!--Breadcrumbs-->
            <div className="text-sm breadcrumbs ml-4 mt-4 w-fit flex flex-col">
               <ul>
                  <li><Link :href="route('dashboard')">Home</Link></li>
                  <li>{{ aircraft.registration }}</li>
               </ul>
            </div>
            <!--Data-->
            <Data />
         </section>
         <!--Map-->
         <Map :plane="live" :flight="flight" />
      </div>
   </AuthenticatedLayout>
</template>
