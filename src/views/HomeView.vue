<template>
  <Navbar />
  <div class="home">home</div>
</template>

<script>
import { mapActions } from "vuex";
import Navbar from "@/components/Header/Navbar.vue";
export default {
  name: "HomeView",
  components: {
    Navbar,
  },
  mounted() {
    let user = localStorage.getItem("user-info");
    if (!user) {
      this.redirectTo({ val: "Sign-up" });
    }
  },
  methods: {
    ...mapActions(["redirectTo"]),
  },
};
</script>
