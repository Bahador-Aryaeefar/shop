<template>
    <div class="border-[2px] rounded-[0.25rem] p-6 text-lg">
        <div class="card-body">
            <h5 class="font-bold">{{ product.name }}</h5>
            <div class="flex gap-6 mt-4">
                <p>Price: <b>${{ product.price }}</b></p>
                <p>Quantity: <b>{{ product.quantity }}</b></p>
            </div>
            <button class="py-1 px-3 text-white rounded-[0.25rem] mt-4"
                :class="inCartQuantity ? 'bg-green-600' : 'bg-red-600'" @click="addToCart(product.id)"
                :disabled="!inCartQuantity">
                {{ inCartQuantity ? "Add to Cart" : "Out of stock" }}
            </button>
        </div>
    </div>
</template>

<script>
export default {
    props: ["product"],
    computed: {
        inCartQuantity() {
            return this.product.quantity;
        }
    },
    methods: {
        addToCart(id) {
            this.$store.dispatch("addToCart", id);
        }
    }
}
</script>