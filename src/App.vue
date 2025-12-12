<template>
  <div id="app" class="container mt-4">
    <h1 class="text-center text-primary">Final</h1>
    
    <LoginComponent 
      v-if="!isLoggedIn" 
      @authenticated="handleLoginSuccess" 
    />
    
    <GridDisplay 
      v-else 
      :items="fetchedData" 
    />
    
  </div>
</template>

<script>
import LoginComponent from './components/LoginComponent.vue';
import GridDisplay from './components/GridDisplay.vue';

export default {
  name: 'App',
  components: {
    LoginComponent,
    GridDisplay
  },
  data() {
    return {
      isLoggedIn: false,
      fetchedData: [],
      apiUrl: 'https://cis255.vercel.app/api' 
    }
  },
  methods: {
    async handleLoginSuccess() {
      await this.fetchData('cardinals'); 
    },
    
    async fetchData(password) {
      try {
        const response = await fetch(this.apiUrl, {
          method: 'POST', 
          headers: {
            'Content-Type': 'application/json' 
          },
          body: JSON.stringify({ password: password }) 
        });
        
        const responseData = await response.json();
        let rawItems = [];
        if (responseData && Array.isArray(responseData.books)) {
            rawItems = responseData.books;
        } else if (responseData && Array.isArray(responseData)) {
            rawItems = responseData;
        } else {
            rawItems = [];
        }

        this.fetchedData = rawItems.map(item => ({
            ...item,
            status: item.year > 1950,
            detail: `Published in ${item.year}` 
        }));
        
        this.isLoggedIn = true; 
        
      } catch (error) {
        this.isLoggedIn = false;
      }
    }
  }
}
</script>

<style>
@import '~bootstrap/dist/css/bootstrap.css';
body {
  background-color: #f8f9fa;
}
</style>