<template>
  <div class="container">
    <div>
      <center>
      <div class="clearfix">
        <b-img
          left
          src="https://static.wixstatic.com/media/85baeb_8a4fe5109ca84fbca03dd071bb05fb38~mv2.png"
          fluid alt="Responsive image"
          height="200px"
          weight="200px"
        ></b-img>
      </div> 
      </center>

      <h1 class="title">Parking</h1>
      <h2 class="subtitle">Publico</h2>
    </div>
  </div>
</template>

<script>
import Logo from "~/components/Logo.vue";

export default {
  components: {
    Logo
  }
};
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
