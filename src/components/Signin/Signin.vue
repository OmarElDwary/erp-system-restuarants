<template>
  <form @click.prevent>
    <div class="row g-3 align-items-center">
      <h1>Sign In</h1>
      <div class="col-auto d-block mx-auto">
        <input
          type="text"
          class="form-control"
          placeholder="Your Email"
          v-model="state.email"
        />
        <span class="error-feedback" v-if="v$.email.$error">
          {{ v$.state.email.$errors[0].$message }}
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
        <span class="error-feedback" v-if="v$.pass.$error">
          {{ v$.state.pass.$errors[0].$message }}
        </span>
      </div>
    </div>
    <br />
    <div class="row g-3 align-items-center">
      <div class="col-auto d-block mx-auto">
        <button type="submit" @click="SignInUser()" class="btn btn-primary">
          Sign In
        </button>
        &nbsp;&nbsp;&nbsp;
        <button
          type="button"
          @click="redirectTo({ val: 'Sign-up' })"
          class="btn btn-primary"
        >
          Sign Up
        </button>
      </div>
    </div>
    <br />
    <div class="col-auto d-block mx-auto text-center error-feedback">
      {{ userNotFound }}
    </div>
  </form>
</template>

<script>
import axios from "axios";
import { mapActions } from "vuex";
import useValidate from "@vuelidate/core";
import { required, email } from "@vuelidate/validators";
import { reactive, computed } from "vue";
export default {
  name: "SignInForm",
  mounted() {
    let user = localStorage.getItem("user-info");
    if (user) {
      this.redirectTo({ val: "HomeView" });
    }
  },
  setup() {
    const state = reactive({
      pass: "",
      email: "",
    });
    const rules = computed(() => {
      return {
        pass: { required },
        email: { required, email },
      };
    });

    const v$ = useValidate(rules, state);
    return {
      state,
      v$,
    };
  },
  data() {
    return {
      userNotFound: "",
    };
  },
  methods: {
    ...mapActions(["redirectTo"]),
    async SignInUser() {
      this.v$.$validate();
      if (!this.v$.$error) {
        let result = await axios.get(
          `http://localhost:3000/users?email=${this.state.email}&pass=${this.state.pass}`
        );
        if (result.status == 200 && result.data.length > 0) {
          localStorage.setItem("user-info", JSON.stringify(result.data[0]));
          this.redirectTo({ val: "HomeView" });
        } else {
          this.userNotFound = "User not found";
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
