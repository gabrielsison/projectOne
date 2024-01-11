<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue'
import { Head, Link } from '@inertiajs/vue3'

const lastUpdated = new Date().toLocaleString()
</script>

<template>
   <Head title="Dashboard" />

   <AuthenticatedLayout>
      <div className="card w-80 m-4 bg-base-100 shadow-xl">
         <figure>
            <img
               src="https://cdn.jetphotos.com/full/6/1064350_1700504515.jpg"
               alt=""
            />
         </figure>
         <div className="card-body">
            <h2 className="card-title">QTR XY123Z</h2>
            <p class="text-sm">Last Updated: {{ lastUpdated }}</p>
            <div className="card-actions justify-end">
               <Link
                  :href="route('plane.view', 1)"
                  className="btn btn-primary btn-wide mx-auto mt-4"
               >
                  Visit
               </Link>
            </div>
         </div>
      </div>
   </AuthenticatedLayout>
</template>
