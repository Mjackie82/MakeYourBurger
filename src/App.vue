<template>
  <div id="nav">
    <Navbar :logo="logoSrc" :alt="appName"/>
    <router-view/>
    <Footer />
  </div>

</template>

<script >
  import Navbar from './components/Navbar.vue'
  import Footer from './components/Footer.vue'

  export default{
    name: "app",
    components:{
      Navbar,
      Footer
    },
    data(){
      return{
        logoSrc: "/img/logo.png",
        appName: "Make Your Burger"
      }
    }
  }
</script>

<style>
  * {
    font-family: Helvetica;
    padding: 0;
    margin: 0;
    box-sizing: border-box;
  }

  .main-container {
    min-height: calc(100vh - 278px);
    width: 100%;
  }

  h1 {
    text-align: center;
    font-size: 42px;
    margin-bottom: 30px;
    color: #222;
  }
</style>
