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
import axios from "axios";

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
  mounted() {
    let user = localStorage.getItem('user-info');
    if(user){
      this.$router.push({ name:"home" })
    }
  },
  methods: {
    login(){
      this.$router.push({ name:"Login" })
    },
    async submit(){
      this.v$.$validate()
      if(!this.v$.$error){
        let result = await axios.post('http://localhost:3000/user',{
          name: this.name,
          email: this.email,
          password: this.password,
        })
        if(result.status === 201){
          alert("SignUp successful")
          localStorage.setItem('user-info',JSON.stringify(result.data))
          this.$router.push({ name:"Login" })
        }
      }
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
