<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue'
import { Head } from '@inertiajs/vue3'
import Map from '@/Components/Map.vue'
import Data from '@/Pages/Partials/Data.vue'
</script>

<template>
   <Head title="Dashboard" />

   <AuthenticatedLayout>
      <div class="flex flex-col-reverse grow">
         <section class="w-full">
            <!--Breadcrumbs-->
            <div className="text-sm breadcrumbs ml-4 mt-4 w-fit flex flex-col">
               <ul>
                  <li><a>Home</a></li>
                  <li>QTR XY123Z</li>
               </ul>
            </div>
            <!--Data-->
            <Data />
         </section>
         <!--Map-->
         <Map />
      </div>
   </AuthenticatedLayout>
</template>
