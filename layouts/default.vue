<template>
  <v-app>
    <MainNavbar />

    <v-content>
      <v-container>
        <router-view>
          <nuxt/>
        </router-view>
      </v-container>
    </v-content>

    <MainFooter />
  </v-app>
</template>

<script>
import MainNavbar from "~/components/MainNavbar.vue";
import MainFooter from "~/components/MainFooter.vue";

export default {
  components: {
    MainNavbar,
    MainFooter
  }
};
</script>

<style lang="stylus">
</style>
