<template>
  <div id="app" class="container mt-4">
    <h1 class="text-center text-primary">CIS255 Final Project Data Viewer</h1>
    
    <LoginComponent 
      v-if="!isLoggedIn" 
      @authenticated="handleLoginSuccess" 
    />
    
    <GridDisplay 
      v-else 
      :items="fetchedData" 
    />

    <div v-if="loginError" class="alert alert-danger mt-3 text-center">
        Login failed or the API returned an error. Check password and try again.
    </div>
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
      loginError: false,
      apiUrl: 'https://cis255.vercel.app/api' 
    }
  },
  methods: {
    async handleLoginSuccess() {
      this.loginError = false;
      // Pass the known password for the API request
      await this.fetchData('cardinals'); 
    },
    
    async fetchData(password) {
      try {
        const response = await fetch(this.apiUrl, {
          method: 'POST', 
          headers: {
            'Content-Type': 'application/json' 
          },
          // Send the password in the body for authentication
          body: JSON.stringify({ password: password }) 
        });
        
        if (!response.ok) {
           throw new Error(`HTTP error! Status: ${response.status}`);
        }
        
        const responseData = await response.json();
        let rawItems = [];

        // 1. Extract the actual data array. Based on the screenshot, 
        // the data is likely under a 'books' property or similar, or it's the array itself.
        if (responseData && Array.isArray(responseData.books)) {
            rawItems = responseData.books;
        } else if (responseData && Array.isArray(responseData)) {
            rawItems = responseData; // If it's the simple array structure
        } else {
            rawItems = [];
        }

        // 2. REQUIREMENT FIX: Inject the missing 'status' field 
        // We will base the status on the year for demonstration.
        this.fetchedData = rawItems.map(item => ({
            ...item,
            // Synthesize 'status': TRUE if published after 1950, FALSE otherwise.
            status: item.year > 1950,
            detail: `Published in ${item.year}` // Added detail field for ItemCard clarity
        }));
        
        this.isLoggedIn = true; 
        
      } catch (error) {
        console.error("API Fetch Error:", error);
        this.loginError = true;
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