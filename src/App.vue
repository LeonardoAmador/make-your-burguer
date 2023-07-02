<template>
  <Navbar :logo="logoSrc" :alt="altName" />
  <router-view />
  <Footer />
</template>

<script>
import Navbar from "./components/Navbar.vue";
import Footer from "./components/Footer.vue";

export default {
  components: {
    Navbar,
    Footer,
  },
  data() {
    return {
      logoSrc: "/img/logo.png",
      alt: "Burguer logo",
    };
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: Helvetica;
}

.mainContainer {
  margin: 50px;
  margin-bottom: 100px;
  min-height: 250px;
  display: flex;
  flex-direction: column;
}

h1 {
  text-align: center;
  font-size: 42px;
  margin-bottom: 30px;
  color: #222;
}
</style>
