<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink, RouterView } from 'vue-router'
import registration from '@/views/registration/registration.vue';
import index from './views/pages/index.vue';



const isLoggedIn$ = ref(false);
onMounted(()=>{
  if (localStorage.getItem('user-info')){
    isLoggedIn$ = ref(true);
  }
})


</script>

<template>
  <index/>
  <!-- <RouterView /> -->
</template>

<style scoped>
</style>
