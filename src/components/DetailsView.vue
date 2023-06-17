<template>
    <div class="border mx-5 rounded bg-transparent">
        <h2>Product Details</h2>
        <p>Name: {{ productDetails.title }}</p>
        <p>Price: {{ productDetails.price }}</p>
        <p>Rating: {{ productDetails.rating }}</p>
        <p>Description: {{ productDetails.description }}</p>
      
    </div>
</template>

<script>
export default {
    props: {
        productDetails: {
            type: Object,
            required: true
        }
    }
};
</script>
