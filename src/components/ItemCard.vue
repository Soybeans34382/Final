<template>
  <div 
    class="card h-100 shadow-sm p-3"
    :class="{ 
      'border-success bg-light-green': item.status, 
      'border-danger bg-light-red': !item.status 
    }"
  >
    <div class="card-body p-0">
      <h5 class="card-title">{{ item.title }}</h5>
      <p class="card-text text-muted">Year: {{ item.year }}</p>
      <p class="card-text">{{ item.detail }}</p>
      
      <span 
        class="badge"
        :class="{'bg-success': item.status, 'bg-danger': !item.status}"
      >
        Status: {{ item.status ? 'TRUE (Post-1950)' : 'FALSE (Pre-1950)' }}
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ItemCard',
  props: {
    item: { 
      type: Object,
      required: true
    }
  }
}
</script>

<style scoped>
.bg-light-green {
  background-color: #e6f7e8; 
}
.bg-light-red {
  background-color: #f8e5e5;
}
.card {
    transition: transform 0.2s;
}
.card:hover {
    transform: translateY(-3px);
}
</style>