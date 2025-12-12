<template>
  <div class="d-flex justify-content-center mt-5">
    <div class="card p-4 shadow-sm" style="width: 300px;">
      <h4 class="card-title text-center">Login</h4>
      <form @submit.prevent="submitLogin">
        <div class="mb-3">
          <label for="password" class="form-label">Password</label>
          <input 
            type="password" 
            class="form-control" 
            id="password" 
            v-model="passwordInput"
            :class="{ 'is-invalid': hasError }"
          >
          <div class="invalid-feedback" v-if="hasError">Wrong password!</div>
        </div>
        <button type="submit" class="btn btn-primary w-100">Login</button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'LoginComponent',
  emits: ['authenticated'],
  data() {
    return {
      passwordInput: 'cardinals', // REQUIREMENT: Default password set
      correctPass: 'cardinals',
      hasError: false
    }
  },
  methods: {
    submitLogin() {
      if (this.passwordInput === this.correctPass) {
        this.hasError = false;
        this.$emit('authenticated'); // Success!
      } else {
        this.hasError = true;
      }
    }
  }
}
</script>
<style scoped>
.login-container { min-height: 80vh; }
</style>