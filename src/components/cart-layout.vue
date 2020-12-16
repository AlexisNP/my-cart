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
            <th
              scope="col"
              @click="sortBasket('category')"
              :class="sortStates.category ? 'sorted-down' : 'sorted-up'"
            >
              Catégorie
            </th>
            <th
              scope="col"
              @click="sortBasket('name')"
              :class="sortStates.name ? 'sorted-down' : 'sorted-up'"
            >
              Produit
            </th>
            <th
              scope="col"
              @click="sortBasket('quantity')"
              :class="sortStates.quantity ? 'sorted-down' : 'sorted-up'"
            >
              Quantité
            </th>
            <th
              scope="col"
              @click="sortBasket('price')"
              :class="sortStates.price ? 'sorted-down' : 'sorted-up'"
            >
              Prix
            </th>
          </tr>
        </thead>
        <tbody>
          <cart-row v-for="(product, index) in basketContent" :key="index" :product="product"/>
        </tbody>
        <tfoot>
          <tr class="bg-dark">
            <td colspan="3" class="text-light font-weight-bold">Prix Total</td>
            <td class="text-light font-weight-bold">{{ basketTotal }}</td>
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
    <cart-form
      @addProduct="onAddProduct"
      @resetBasket="onResetBasket"
    />
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
      ],
      sortStates: {
        category: false,
        name: false,
        quantity: false,
        price: false,
      }
    }
  },

  computed: {
    basketContent() {
      return this.basket;
    },
    basketTotal() {
      let totalPrice = 0;
      this.basket.forEach(element => {
        totalPrice += element.quantity * element.price
      });
      return totalPrice + this.currency
    }
  },

  methods: {
    onAddProduct(product) {
      this.basket.push(product);
    },
    onResetBasket() {
      this.basket = [];
    },
    sortBasket(method) {
      let basket = this.basket;

      console.log();

      switch (method) {
        case 'category':
          this.sortStates.category =! this.sortStates.category;
          if (!this.sortStates.category) {
            return basket.sort((a, b) => a.category.localeCompare(b.category));
          } else {
            return basket.sort((a, b) => b.category.localeCompare(a.category));
          }

        case 'name':
          this.sortStates.name =! this.sortStates.name;
          if (!this.sortStates.name) {
            return basket.sort((a, b) => a.libelle.localeCompare(b.libelle));
          } else {
            return basket.sort((a, b) => b.libelle.localeCompare(a.libelle));
          }

        case 'quantity':
          this.sortStates.quantity =! this.sortStates.quantity;
          if (!this.sortStates.quantity) {
            return basket.sort((a, b) => a.quantity - b.quantity);
          } else {
            return basket.sort((a, b) => b.quantity - a.quantity);
          }
      
        case 'price':
          this.sortStates.price =! this.sortStates.price;
          if (!this.sortStates.price) {
            return basket.sort((a, b) => a.price - b.price);
          } else {
            return basket.sort((a, b) => b.price - a.price);
          }

        default:
          return basket;
      }
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

  .cart-body {
    .table {
      thead {
        tr {
          th {
            position: relative;
            cursor: pointer;
            &:after {
              display: block;
              position: absolute;
              top: 50%;
              right: 40px;
              transform: translate(-50%, -50%);
              font-family: 'Material Icons Round';
            }
            &.sorted-down {
              &:after {
                content: 'keyboard_arrow_down'
              }
            }
            &.sorted-up {
              &:after {
                content: 'keyboard_arrow_up'
              }
            }
          }
        }
      }
    }
  }
}
</style>
