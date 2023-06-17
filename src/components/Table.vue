<template>
  <div class="m-0 p-0">

    <div class="filters header py-2">

      <h1 class="text center">Filter data by search</h1>
      <label>Rating:</label> 
      <input class="rounded px-2 " type="number" v-model="ratingFilter"  min="0" max="5"> &nbsp;
      <label>Price:</label>
      <input  class="rounded px-2 " type="number" v-model="priceFilter" min="0"> &nbsp;
      <label>Name:</label>
      <input  class="rounded px-2 " type="text" v-model="nameFilter">
    </div>

    <table class="table table-bordered">
      <thead>
        <tr>
          <th>Serial</th>
          <th>Title</th>
          <th>Price</th>
          <th>Rating</th>
          <th>Action</th>
        </tr>
      </thead>
    <tbody>
          <TableRow
            v-for="product in displayedProducts"
            :key="product.id"
            :product="product"
            @show-details="toggleDetailsView"
          />
        </tbody>
    </table>

    <DetailsView v-if="selectedProductDetails" :productDetails="selectedProductDetails" />

    <Pagination :currentPage="currentPage" :totalPages="totalPages" @page-changed="changePage"></Pagination>
  </div>
</template>

<script>
import axios from 'axios';
import TableRow from './TableRow.vue';
import Pagination from './Pagination.vue';
import DetailsView from './DetailsView.vue';

export default {
  components: {
    TableRow,
    Pagination,
    DetailsView
  },
  data() {
    return {
      products: [],
      currentPage: 1,
      itemsPerPage: 10,
      selectedProductDetails: null,
      ratingFilter: null,
      priceFilter: null,
      nameFilter: '',
      selectedProductId: null
    };
  },
  computed: {
    filteredProducts() {
      let filtered = this.products;

      // Filter by rating
      if (this.ratingFilter !== null) {
        filtered = filtered.filter(product => product.rating >= this.ratingFilter);
      }

      // Filter by price
      if (this.priceFilter !== null) {
        filtered = filtered.filter(product => product.price >= this.priceFilter);
      }

      // Filter by name
      if (this.nameFilter !== '') {
        const nameRegex = new RegExp(this.nameFilter, 'i');
        filtered = filtered.filter(product => nameRegex.test(product.title));
      }

      return filtered;
    },
    displayedProducts() {
      const startIndex = (this.currentPage - 1) * this.itemsPerPage;
      const endIndex = startIndex + this.itemsPerPage;
      return this.filteredProducts.slice(startIndex, endIndex);
    },
    totalPages() {
      return Math.ceil(this.filteredProducts.length / this.itemsPerPage);
    }
  },
  created() {
    this.fetchProducts();
  },
  methods: {
    fetchProducts() {
      axios.get('https://dummyjson.com/products')
        .then(response => {
          this.products = response.data.products;
        })
        .catch(error => {
          console.error(error);
        });
    },
    showDetails(productId) {
      axios.get(`https://dummyjson.com/products/${productId}`)
        .then(response => {
          this.selectedProductDetails = response.data;
        })
        .catch(error => {
          console.error(error);
        });
    },

    toggleDetailsView(productId) {
      const product = this.products.find(p => p.id === productId);
      if (product) {
        if (product.showDetails) {
          // Close the details view if it is already open
          product.showDetails = false;
          this.selectedProductId = null;
        } else {
          // Close all other details views and open the selected one
          this.products.forEach(p => {
            p.showDetails = false;
          });
          product.showDetails = true;
          this.selectedProductId = productId;
        }
      }
    },
    changePage(page) {
      this.currentPage = page;
    }
  }
};
</script>
