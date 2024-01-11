<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue'
import { Head } from '@inertiajs/vue3'
import Map from '@/Components/Map.vue'

const handleClick = () => {
   alert(1)
}
</script>

<template>
   <Head title="Dashboard" />

   <AuthenticatedLayout>
      <template #header>
         <h2 class="font-semibold text-xl text-gray-800 leading-tight">
            Dashboard
         </h2>
      </template>

      <Map />
   </AuthenticatedLayout>
</template>
