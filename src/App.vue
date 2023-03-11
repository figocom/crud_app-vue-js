<template>
  <Products/>
  <Types v-bind:func="getAllProducts"/>
</template>

<script>
import Products from './components/Products.vue'
import Types from './components/Types.vue'


export default {
  name: 'App',

  components: {
    Products,
    Types
  },
  methods:{

  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
