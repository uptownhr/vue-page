<template>
  <div>
    <nav class="navbar">
      <div class="navbar-brand">
        <nuxt-link to="/" class="navbar-item">
          Page Component
        </nuxt-link>
      </div>
      <div class="navbar-menu">
        <div class="navbar-start">
          <nuxt-link to="/about/" class="navbar-item">About</nuxt-link>
        </div>
      </div>
    </nav>
    <nuxt/>
  </div>
</template>

<script type="text/ecmascript-6">
  import Vue from 'vue'
  import Buefy from 'buefy'
  import 'buefy/dist/buefy.css'

  Vue.use(Buefy)

  export default {

  }
</script>