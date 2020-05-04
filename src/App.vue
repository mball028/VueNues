<template>
  <div id="app">
    <Navbar />
    <Body />
    <Footer />
  </div>
</template>

<script>
import Navbar from "./components/Navbar";
import Body from "./components/Body";
import Footer from "./components/Footer";

export default {
  name: "App",
  components: {
    Navbar,
    Body,
    Footer
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0;
  padding: 0;
}
</style>
