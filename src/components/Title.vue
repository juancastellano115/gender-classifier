<template>
  <section class="hero is-primary">
  <div class="hero-body">
    <div class="container">
      <h1 class="title">{{ msg }}</h1>
      <h2 class="subtitle">
       <a href="github.com/juancastellano115"> <i class="fab fa-github"></i> See the code on github </a>
      </h2>
    </div>
  </div>
</section>
</template>

<script>
export default {
  name: "Title",
  props: {
    msg: String
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
   color: #2c3e50;
}
</style>
