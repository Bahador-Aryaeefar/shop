<template>
    <div class="mt-10 py-2 w-[60%] mx-auto">
        <h1 class="text-[3rem] font-bold">Cart <span
                class="bg-[#868e96] rounded-[0.25rem] px-3 py-1 text-white text-[2.5rem] ml-3">{{ cartSize }}
                products</span>
        </h1>
        <div v-if="!cartSize" class="bg-[#e7e8ea] my-5 px-4 py-2 rounded-[0.25rem] border-[2px] border-[#dddfe2]">
            Cart is empty! Please add some products.
        </div>
        <table v-else class="w-full mb-5 mt-10 text-center">
            <thead class="thead-dark">
                <tr class="bg-[#212529] text-white text-xl h-16 font-bold">
                    <th scope="col">#</th>
                    <th scope="col">Product</th>
                    <th scope="col">Price</th>
                    <th scope="col">Quantity</th>
                    <th scope="col">Fast Shipping</th>
                    <th scope="col">Delete</th>
                    <th scope="col">Total</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(product, index) in cart" :key="product.id" class="odd:bg-[rgba(0,0,0,.05)] text-xl h-16">
                    <th class="pl-2 font-bold">{{ index + 1 }}</th>
                    <td class="">{{ product.name }}</td>
                    <td>$ {{ product.price }}</td>
                    <td>
                        <button @click="removeFromCart(product.id)" :disabled="product.quantity === 1"
                            class="border-[2px] border-red-600 rounded-[0.25rem] px-3 py-1 text-red-600">
                            -
                        </button>
                        <span class="mx-4">{{ product.quantity }}</span>
                        <button @click="addToCart(product.id)" :disabled="product.quantity === product.stock"
                            class="border-[2px] border-green-600 rounded-[0.25rem] px-3 py-1 text-green-600">
                            +
                        </button>
                    </td>
                    <!-- Decorator Pattern: fast sheeping is and add on for cart items -->
                    <td>
                        <input type="checkbox" class="w-6 h-6 cursor-pointer" @click.prevent="changeShipping(product.id)"
                            :checked="product.fast">
                    </td>
                    <td><button @click="deleteFromCart(product.id)"
                            class="px-3 py-1.5 bg-red-600 text-white rounded-[0.25rem]">
                            Delete
                        </button></td>
                    <td>$ {{ product.quantity * product.price }}</td>
                </tr>

                <tr class="bg-[#212529] text-white text-xl h-16 font-bold">
                    <td colspan="6" class="text-right">Total:</td>
                    <td>$ {{ cartTotalAmount }}</td>
                </tr>
            </tbody>
        </table>


        <!-- strategy pattern: different payment strategies -->
        <div class="text-[3rem] font-bold mt-16">
            Payment Method
        </div>

        <div class="flex gap-6 text-2xl mt-5">
            <div class="flex gap-2 items-center">
                <input class="w-5 h-5 cursor-pointer" type="radio" id="credit" name="payment" value="credit" v-model="method">
                <label class="cursor-pointer" for="credit">Credit</label><br>
            </div>
            <div class="flex gap-2 items-center">
                <input class="w-5 h-5 cursor-pointer" type="radio" id="cash" value="cash" name="payment" v-model="method">
                <label class="cursor-pointer" for="cash">Cash</label><br>
            </div>
        </div>

        <button class="py-1 px-3 text-white rounded-[0.25rem] mt-5 bg-green-600 text-2xl mb-10"
            @click="pay()">Checkout</button>
    </div>
</template>
 
<script>
import { mapState, mapGetters } from 'vuex'
export default {
    data() {
        return {
            method: "credit"
        };
    },
    computed: {
        ...mapState([
            "cart"
        ]),
        ...mapGetters([
            "cartSize",
            "cartTotalAmount"
        ])
    },
    methods: {
        addToCart(id) {
            this.$store.dispatch("addToCart", id);
        },
        removeFromCart(id) {
            this.$store.dispatch("removeFromCart", id);
        },
        deleteFromCart(id) {
            this.$store.dispatch("deleteFromCart", id);
        },
        changeShipping(id) {
            this.$store.dispatch("changeShipping", id);
        },
        pay() {
            // strategy pattern: different payment strategies
            if(this.method == "credit") this.$store.state.credit -= this.cartTotalAmount;
            else  this.$store.state.cash -= this.cartTotalAmount;
            this.$store.state.cart = []
        }
    }
} 
</script>