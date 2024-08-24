<template>
  <form @click.prevent>
    <div class="row g-3 mb-2 align-items-center text-center">
      <h1>Sign Up</h1>
      <div class="col-auto d-block mx-auto">
        <input
        type="text"
        class="form-control"
        placeholder="Enter Your Name"
        v-model="name"
        />
      </div>
      <span class="error-feedback" v-if="v$.name.$error">{{ v$.name.$errors[0].$message }}</span>
    </div>
    <div class="row g-3 mb-2 align-items-center text-center">
      <div class="col-auto d-block mx-auto ">
        <input
          type="text"
          class="form-control"
          placeholder="Enter Your Email"
          v-model="email"
        />
        <span class="error-feedback" v-if="v$.email.$error">{{ v$.email.$errors[0].$message }}</span>
      </div>
    </div>
    <div class="row g-3 mb-2 align-items-center text-center">
      <div class="col-auto d-block mx-auto ">
        <input
          type="password"
          class="form-control"
          placeholder="Enter Your password"
          v-model="password"
        />
      </div>
      <span class="error-feedback" v-if="v$.password.$error">{{ v$.password.$errors[0].$message }}</span>
    </div>
    <div class="row g-3 align-items-center">
      <div class="col-auto d-block mx-auto text-center">
        <button type="submit" @click="submit()" class="btn btn-primary">SignUp</button><br/>
        <button type="button" class="btn btn-link" @click="login()">Already have an account? Login</button>
      </div>
    </div>
  </form>
</template>

<script>
import useValidate from "@vuelidate/core"
import { required,email,minLength } from "@vuelidate/validators"

export default {
  name: "SignUpForm",
  data() {
    return {
      v$: useValidate(),

      name: "",
      email: "",
      password: "",
    };
  },
  validations() {
    return {
      name: {
        required,minLength:minLength(4)
      },
      email: {
        required,email,
      },
      password: {
        required,minLength:minLength(8)
      },
    }
  },
  methods: {
    login(){
      this.$router.push({ name:"Login" })
    },
    submit(){
      this.v$.$validate()
      // if(!this.v$.$error){
      //   alert("Success!")
      // }
      // else{
      //   alert(`you must enter data ${this.v$.$errors[0].$message}`)
      // }
    },
  },
};
</script>
<style>

.error-feedback {
  color: red;
}
</style>
