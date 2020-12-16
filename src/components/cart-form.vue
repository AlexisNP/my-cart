<template>
  <div class="cart-form">
    <h2 class="font-weight-bold mb-4">{{ header.title }}</h2>
    <form @submit="addProduct">
      <div class="form-row">
        <div class="form-group col-md-6">
          <label for="category">Catégorie</label>
          <select class="custom-select" v-model="activeCategory">
            <option disabled selected value="false">Choisir une catégorie</option>
            <option
              v-for="(category, index) in listCategories"
              :key="index"
              :value="category"
            >
              {{ category.category }}
            </option>
          </select>
        </div>
        <div class="form-group col-md-6">
          <label for="product">Produit</label>
          <span v-if="activeCategory">
            <select class="custom-select" v-model="activeProduct">
              <option disabled selected value="false">Choisir un produit</option>
              <option
                v-for="(product, index) in activeCategory.products"
                :key="index"
                :value="product"
              >
                {{ product.libelle }}
              </option>
            </select>
          </span>
          <span v-else>
            <input
              class="form-control"
              type="text"
              placeholder="Selectionnez une catégorie"
              readonly
            >
          </span>
        </div>
        <div class="form-group col-md-6">
          <input
            type="number"
            class="form-control"
            placeholder="Choisissez une quantité"
            min="1"
            max="999"
            v-model="activeProductQuantity"
          >
        </div>
        <div class="form-group col-md-3">
          <input
            type="reset"
            class="btn btn-outline-secondary btn-block"
            value="Réinitialiser"
            @click="resetBasket()"
          >
        </div>
        <div class="form-group col-md-3">
          <input 
            type="submit" 
            class="btn btn-primary btn-block"
            value="Ajouter"
            :disabled="checkProduct()"
          >
        </div>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: 'cart-form',
  data() {
    return {
      header: {
        title: "Ajouter un produit"
      },
      products: [
        {
          category: "Epicerie salée",
          products: [
            {
              libelle: "Moutarde",
              price: 2.45
            },
            { 
              libelle: "Ketchup",
              price: 4.87
            },
            {
              libelle: "Herbes de Provence",
              price: 1.78
            },
            {
              libelle: "Mayonnaise",
              price: 2.98
            }, 
            {
              libelle: "Curry",
              price: 5.76
            }, 
            {
              libelle: "Huile d'olive",
              price: 4.23
            }
          ]
        },
        {
          category: "Epicerie sucrée",
          products: [
            {
              libelle: "Biscuits au chocolat",
              price: 2.45
            },
            { 
              libelle: "Céréales",
              price: 4.87
            },
            {
              libelle: "Bonbons",
              price: 1.78
            },
            {
              libelle: "Chocolats",
              price: 2.98
            }, 
            {
              libelle: "Confiture",
              price: 5.76
            }, 
            {
              libelle: "Pâte à tartiner",
              price: 4.23
            }
          ]
        },
        {
          category: "Produits laitiers",
          products: [
            {
              libelle: "Fromage de chèvre",
              price: 2.45
            },
            { 
              libelle: "Yaourts au citron",
              price: 4.87
            },
            {
              libelle: "Emmental",
              price: 1.78
            },
            {
              libelle: "Comté",
              price: 2.98
            }, 
            {
              libelle: "Lait écrémé",
              price: 5.76
            }, 
            {
              libelle: "Oeufs",
              price: 4.23
            }
          ]
        },
        {
          category: "Fruits et Légumes",
          products: [
            {
              libelle: "Bananes",
              price: 2.45
            },
            { 
              libelle: "Oranges",
              price: 4.87
            },
            {
              libelle: "Concombre",
              price: 1.78
            },
            {
              libelle: "Tomates",
              price: 2.98
            }, 
            {
              libelle: "Pommes de terre",
              price: 5.76
            }, 
            {
              libelle: "Framboises",
              price: 4.23
            }
          ]
        },
        {
          category: "Produits ménagers",
          products: [
            {
              libelle: "Eponge",
              price: 2.45
            },
            { 
              libelle: "Papier toilette",
              price: 4.87
            },
            {
              libelle: "Mouchoirs",
              price: 1.78
            },
            {
              libelle: "Liquide vaisselle",
              price: 2.98
            }, 
            {
              libelle: "Lessive",
              price: 5.76
            }, 
            {
              libelle: "Papier aluminium",
              price: 4.23
            }
          ]
        },
        {
          category: "Boucherie / Charcuterie",
          products: [
            {
              libelle: "Saucisson",
              price: 2.45
            },
            { 
              libelle: "Tranches de jambon",
              price: 4.87
            },
            {
              libelle: "Poulet rôti",
              price: 1.78
            },
            {
              libelle: "Côtes de porc",
              price: 2.98
            }, 
            {
              libelle: "Steak haché",
              price: 5.76
            }, 
            {
              libelle: "Escalopes de dinde",
              price: 4.23
            }
          ]
        },
      ],
      activeCategory: false,
      activeProduct: false,
      activeProductQuantity: 1,
    }
  },
  computed: {
    listCategories() {
      let categories = []
      this.products.forEach(c => {
        categories.push(c)
      });
      return categories
    },
  },
  methods: {

    // Resets the form and props
    resetForm() {
      this.activeCategory = false;
      this.activeProduct = false;
      this.productQuantity = false;
    },

    // Resets basket
    resetBasket() {
      this.resetForm();
      this.$emit('resetBasket');
    },

    // Checks if the product is valid for sending to list
    checkProduct() {
      if (
        !this.activeCategory ||
        !this.activeProduct ||
        this.activeProductQuantity < 1
      ) {
        return true
      }
      return false
    },

    // Emits event to sibling component
    addProduct(e) {
      let product = {};
        product.libelle = this.activeProduct.libelle;
        product.price = this.activeProduct.price;
        product.category = this.activeCategory.category;
        product.quantity = this.activeProductQuantity;
      this.$emit('addProduct', product);
      this.resetForm();
      e.preventDefault();
    }
  },
}
</script>

<style>

</style>