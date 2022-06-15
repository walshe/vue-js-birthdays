<script>
import { useRoute } from "vue-router";

export default {

  
};
</script>

<template>
  <main>
    
    <p>Name: {{ $route.params.name }}</p>
    <p>Age: {{ $route.params.age }}</p>
    <br/>
    <button @click="$router.go(-1)">Back </button>
    
    
  </main>
</template>
