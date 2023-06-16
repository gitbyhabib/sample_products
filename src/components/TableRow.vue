<template>
  <tr>
    <td>{{ product.id }}</td>
    <td>{{ product.title }}</td>
    <td>{{ product.price }}</td>
    <td>{{ product.rating }}</td>
    <td>
      <button
        :class="{
          'btn btn-success': !product.showDetails,
          'btn btn-danger': product.showDetails
        }"
        @click="toggleDetailsView"
      >
        {{ product.showDetails ? 'Close' : 'Show' }}
      </button>
    </td>
  </tr>
  <tr v-if="product.showDetails">
    <td colspan="4">
      <DetailsView :productDetails="product"></DetailsView>
    </td>
  </tr>
</template>

<script>
import DetailsView from './DetailsView.vue';

export default {
  props: {
    product: {
      type: Object,
      required: true
    }
  },
  components: {
    DetailsView
  },
  emits: ['show-details'],
  methods: {
    toggleDetailsView() {
      this.$emit('show-details', this.product.id);
    }
  }
};
</script>
