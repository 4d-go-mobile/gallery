<template>
  <div id="app">
    <Header v-if="picker"/>
    <div v-if="picker" class="color-bg-inset navPusher">
      <router-view></router-view>
    </div>
    <div v-else class="color-bg-inset p-3">
      <router-view></router-view>
    </div>
    <Footer v-if="picker"/>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";

export default {
  props: ["type"],
  data() {
    return {
      picker: this.$route.params.picker == undefined
    };
  },
  components: {
    Header,
    Footer
  }
};
</script>
<style lang="sass">
$marketing-font-path: "/node_modules/@primer/css/fonts/" !default;
@import "@primer/css/index.scss"
</style>
<style scoped>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}

</style>