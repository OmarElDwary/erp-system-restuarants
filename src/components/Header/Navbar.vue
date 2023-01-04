<template>
  <ul id="nav">
    <router-link
      li
      :to="{ name: 'HomeView', params: { pageTitle: 'HomeView' } }"
      exact
    >
      <a>Home</a>
    </router-link>
    <a href="#" @click="SignMeOut()">Log</a>
  </ul>
</template>

<script>
import { mapActions } from "vuex";
export default {
  name: "Nav-bar",
  methods: {
    ...mapActions(["redirectTo"]),
    SignMeOut() {
      localStorage.clear();
      this.redirectTo({ val: "Sign-in" });
    },
  },
};
</script>

<style lang="scss" scoped>
$fontColor: darkcyan;
$navColor: #525a5a;

ul {
  display: flex;
  justify-content: center;
  list-style: none;
  margin-bottom: 50px;

  a {
    text-decoration: none;
    font-weight: bold;
    color: $fontColor;
    padding: 10px;
  }
}

a.router-link-exact-active a,
ul,
a a:hover {
  color: $navColor;
  box-shadow: 2px 2px 4px 0 $navColor;
}
</style>
