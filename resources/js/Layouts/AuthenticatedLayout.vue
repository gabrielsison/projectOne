<script setup>
import Navbar from '@/Components/Navbar.vue'
</script>

<template>
   <div>
      <div class="min-h-screen bg-gray-100 flex flex-col">
         <Navbar />
         <!-- Page Content -->
         <main class="grow flex flex-col">
            <slot />
         </main>
      </div>
   </div>
</template>
