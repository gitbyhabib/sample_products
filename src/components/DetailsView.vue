<template>
    <div>
        <h2>Product Details</h2>
        <p>Name: {{ productDetails.title }}</p>
        <p>Price: {{ productDetails.price }}</p>
        <p>Rating: {{ productDetails.rating }}</p>
        <!-- Add more details as needed -->
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
