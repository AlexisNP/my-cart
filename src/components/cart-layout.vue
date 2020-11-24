<template>
  <div class="cart">
    <div class="cart-header">
      <h1 class="title">{{ header.title }}</h1>
      <h3 class="subtitle">{{ header.subtitle }}</h3>
    </div>
    <div class="cart-body">
      <table class="table">
        <thead class="thead-dark">
          <tr>
            <th scope="col">Catégorie</th>
            <th scope="col">Produit</th>
            <th scope="col">Quantité</th>
            <th scope="col">Prix</th>
          </tr>
        </thead>
        <tbody>
          <cart-row v-for="(product, index) in basket" :key="index" :product="product"/>
        </tbody>
        <tfoot>
          <tr class="bg-dark">
            <td colspan="3" class="text-light font-weight-bold">Prix Total</td>
            <td class="text-light font-weight-bold">{{ totalBasket }}</td>
          </tr>
        </tfoot>
      </table>
    </div>
    <div class="cart-actions">
      <div class="text-right">
        <button type="button" class="btn btn-outline-secondary">Annuler</button>
        <button type="button" class="btn btn-primary">Valider</button>
      </div>
    </div>
    <hr>
    <cart-form />
  </div>
</template>

<script>

import CartRow from '@/components/cart-row'
import CartForm from '@/components/cart-form'

export default {
  name: 'cart',
  components: {
    'cart-row': CartRow,
    'cart-form': CartForm
  },
  data() {
    return {
      header: {
        title: "Mon Panier",
        subtitle: "Je prépare mon panier pour la semaine"
      },
      currency: "€",
      basket: [
        {
          category: "Epicerie salée",
          libelle: "Moutarde",
          quantity: 4,
          price: 1.40
        },
        {
          category: "Fruits et Légumes",
          libelle: "Bananes",
          quantity: 1,
          price: 1.99
        },
        {
          category: "Produits laitiers",
          libelle: "Yaourts au citron",
          quantity: 6,
          price: 1.14
        }
      ]
    }
  },
  computed: {
    totalBasket() {
      let totalPrice = 0;
      this.basket.forEach(element => {
        totalPrice += element.quantity * element.price
      });
      return totalPrice + this.currency
    }
  },
}
</script>

<style lang="scss" scope>
.cart {
  margin-bottom: 40px;

  .cart-header {
    padding-bottom: 20px;
    text-align: center;
    
    .title {
      font-size: 48px;
      font-weight: 700;

    }
    .subtitle {
      font-size: 36px;
      font-weight: 700;
    }
  }
}
</style>
