<template>
    <div>
        <button @click="changePage(currentPage - 1)" :disabled="currentPage === 1">Previous</button>
        <span>{{ currentPage }}</span>
        <button @click="changePage(currentPage + 1)" :disabled="currentPage === totalPages">Next</button>
    </div>
</template>

<script>
export default {
    props: {
        currentPage: {
            type: Number,
            required: true
        },
        totalPages: {
            type: Number,
            required: true
        }
    },
    methods: {
        changePage(page) {
            this.$emit('page-changed', page);
        }
    }
};
</script>
