<template>
  <div id="app">
    <cart/>
  </div>
</template>

<script>
import CartLayout from './components/cart-layout'

export default {
  name: 'App',
  components: {
    'cart': CartLayout
  }
}
</script>

<style lang="scss">
#app {
  padding: 25px;
}

.btn {
  & ~ & {
    margin-left: 20px;
  }
}

</style>
