<template>
    <div class="container">
        <LoginForm/>
    </div>
</template>

<script>
import { LoginForm } from "../components/Signup/Login.vue";
export default {
    name: "LoginForm",
    components: {
        LoginForm,
    }
};
</script>