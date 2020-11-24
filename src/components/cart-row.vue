<template>
  <tr>
    <th scope="row">{{ product.category }}</th>
    <td>{{ product.libelle }}</td>
    <td>{{ product.quantity }}</td>
    <td>{{ productPrice }}</td>
  </tr>
</template>

<script>
export default {
  name: 'cart-row',
  data() {
    return {
      currency: "€"
    }
  },
  props: {
    product: {
      libelle: String,
      category: String,
      quantity: Number,
      price: Number,
    }
  },
  computed: {
    productPrice() {
      return this.product.quantity * this.product.price + this.currency
    }
  },
}
</script>

<style>

</style>