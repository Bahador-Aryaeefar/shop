<template>   
    <div class="mt-10 py-2 w-[60%] mx-auto">
  		        <h1 class="text-[3rem] font-bold">Products</h1>
       <div class="grid gap-10 mt-10" style="grid-template-columns: 1fr 1fr 1fr;">
           <div v-for="product in products" :key="product.id" class="col-sm-6 col-md-4 p-2">
               <single-product :product="product"></single-product>
           </div>
       </div>
   </div>
</template>   

<script>
    import SingleProduct from "./SingleProduct.vue"
    export default{
        components: {
            SingleProduct
        },
        computed: {
            products(){
                return this.$store.state.products;
            }
        },
        created(){
            this.$store.dispatch("fetchProducts");
        }
    }
</script>