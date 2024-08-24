<template>
  <form @click.prevent>
    <div class="row g-3 mb-2 align-items-center text-center">
      <h1>Login</h1>
      <div class="col-auto d-block mx-auto">
        <input
          type="text"
          class="form-control"
          placeholder="Enter Your Email"
          v-model="email"
        />
      </div>
      <span class="error-feedback" v-if="v$.email.$error">{{ v$.email.$errors[0].$message }}</span>
    </div>
    <div class="row g-3 mb-2 align-items-center text-center">
      <div class="col-auto d-block mx-auto">
        <input
          type="password"
          class="form-control"
          placeholder="Enter Your password"
          v-model="password"
        />
      </div>
      <span class="error-feedback" v-if="v$.password.$error " >{{ v$.password.$errors[0].$message }}</span>
    </div>
    <div class="row g-3 mb-2 align-items-center">
      <div class="col-auto d-block mx-auto text-center">
        <button type="submit" @click="login()" class="btn btn-primary">Login</button> <br>
        <button type="button" class="btn btn-link" @click="signup()">You don't have an accont? create one</button>
      </div>
    </div>
  </form>
</template>

<script>
import useVuelidate from '@vuelidate/core';
import { required,email } from '@vuelidate/validators';

export default {
  name: "LoginForm",
  data() {
    return {
      v$: useVuelidate(),
      email: "",
      password: "",
    };
  },
  validations(){
    return {
      email: { required,email },
      password: { required },
    };
  },
  methods: {
    signup(){
      this.$router.push({ name:"Signup" })
    },
    login(){
      this.v$.$validate()
    },
  },
};
</script>
