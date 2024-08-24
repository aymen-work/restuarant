<template>
  <form @click.prevent>
    <div class="row g-3 mb-2 align-items-center">
      <h1>Login</h1>
      <div class="col-auto d-block mx-auto">
        <input
          type="text"
          class="form-control"
          placeholder="Enter Your Email"
          v-model="email"
        />
      </div>
    </div>
    <div class="row g-3 mb-2 align-items-center">
      <div class="col-auto d-block mx-auto">
        <input
          type="password"
          class="form-control"
          placeholder="Enter Your password"
          v-model="password"
        />
      </div>
    </div>
    <div class="row g-3 mb-2 align-items-center">
      <div class="col-auto d-block mx-auto text-center">
        <button type="submit" class="btn btn-primary">Login</button> <br>
        <button type="button" class="btn btn-link" @click="signup()">You don't have an accont? create one</button>
      </div>
    </div>
  </form>
</template>

<script>
export default {
  name: "LoginForm",
  data() {
    return {
      email: "",
      password: "",
    };
  },
  methods: {
    signup(){
      this.$router.push({ name:"Signup" })
    }
  },
};
</script>
