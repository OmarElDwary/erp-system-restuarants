<template>
  <form @click.prevent>
    <div class="row g-3 align-items-center">
      <h1>Sign Up Form</h1>
      <div class="col-auto d-block mx-auto">
        <input
          type="text"
          class="form-control"
          placeholder="Your Name"
          v-model="state.name"
        />
        <span class="error-feedback" v-if="v$.name.$error">
          {{ v$.name.$errors[0].$message }}
        </span>
      </div>
    </div>
    <br />
    <div class="row g-3 align-items-center">
      <div class="col-auto d-block mx-auto">
        <input
          type="text"
          class="form-control"
          placeholder="Your Email"
          v-model="state.email"
        />
        <span class="error-feedback" v-if="v$.name.$error">
          {{ v$.email.$errors[0].$message }}
        </span>
      </div>
    </div>
    <br />
    <div class="row g-3 align-items-center">
      <div class="col-auto d-block mx-auto">
        <input
          type="password"
          class="form-control"
          placeholder="Your Password"
          v-model="state.pass"
        />
        <span class="error-feedback" v-if="v$.name.$error">
          {{ v$.pass.$errors[0].$message }}
        </span>
      </div>
    </div>
    <br />
    <div class="row g-3 align-items-center">
      <div class="col-auto d-block mx-auto">
        <button type="submit" @click="SignUpNow()" class="btn btn-primary">
          Sign up Now
        </button>
        &nbsp;&nbsp;&nbsp;
        <button
          type="button"
          @click="redirectTo({ val: 'Sign-in' })"
          class="btn btn-primary"
        >
          Sign In
        </button>
      </div>
    </div>
    <br />
  </form>
</template>

<script>
import axios from "axios";
import { mapActions } from "vuex";
import useValidate from "@vuelidate/core";
import { required, email } from "@vuelidate/validators";
export default {
  name: "SignUpForm",
  data() {
    return {
      v$: useValidate(),
      name: "",
      pass: "",
      email: "",
    };
  },
  validations() {
    return {
      name: { required },
      pass: { required },
      email: { required, email },
    };
  },
  mounted() {
    let user = localStorage.getItem("user-info");
    if (user) {
      this.redirectTo({ val: "HomeView" });
    }
  },
  methods: {
    ...mapActions(["redirectTo"]),
    async SignUpNow() {
      this.v$.$validate();
      if (!this.v$.$error) {
        let result = await axios.post("http://localhost:3000/users", {
          name: this.name,
          pass: this.pass,
          email: this.email,
        });
        if (result.status == 201) {
          localStorage.setItem("user-info", JSON.stringify(result.data));
          console.log(JSON.stringify(result.data));
          this.redirectTo({ val: "HomeView" });
        }
      }
    },
  },
};
</script>
<style scoped>
.error-feedback {
  color: red;
  font-size: 0.9rem;
}
</style>
