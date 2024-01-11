<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue'
import { Head } from '@inertiajs/vue3'
import Map from '@/Components/Map.vue'
import Data from '@/Pages/Partials/Data.vue'
</script>

<template>
   <Head title="Dashboard" />

   <AuthenticatedLayout>
      <div class="flex flex-col-reverse md:flex-row grow">
         <Data />
         <Map />
      </div>
   </AuthenticatedLayout>
</template>
