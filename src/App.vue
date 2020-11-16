<template>
  <v-app>
    <v-main>
      <Nav />
      <Login />
    </v-main>
  </v-app>
</template>

<script>
import Login from "./components/Login.vue";
import Nav from "./components/Nav.vue";

export default {
  name: "App",

  components: {
    Login,
    Nav
  },

  data: () => ({
    //
  })
};
</script>
