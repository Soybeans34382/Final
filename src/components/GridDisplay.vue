<template>
  <div class="mt-4">
    <h3 class="text-center mb-3">Data Results ({{ items.length }} total)</h3>
    
    <div class="row g-3"> 
      
      <div 
        v-for="item in items" 
        :key="item.id" 
        class="col-12 col-md-6 col-lg-4" 
      >
        <ItemCard :item="item" /> 
      </div>
      
    </div>
  </div>
</template>

<script>
import ItemCard from './ItemCard.vue';

export default {
  name: 'GridDisplay',
  components: {
    ItemCard
  },
  props: {
    items: Array 
  }
}
</script>