<template>
  <nav class="bg-[#343a40] py-4 px-6 w-full text-white text-2xl flex gap-6">
    <a class="navbar-brand" href="#"><b>Bahador Aryaeefar</b></a>

    <div class="">Credit: ${{ state.credit }}</div>

    <div>Cash: ${{ state.cash }}</div>
  </nav>
</template> 

<script>
    export default{
        computed: {
            state(){
                return this.$store.state;
            }
        }
    }
</script>   