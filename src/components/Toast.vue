<template>
    <div class="alert w-[30%] text-center bg-[rgba(0,0,0,0.7)] font-bold text-white fixed left-[50%] top-[5rem] py-2 rounded-[0.25rem] text-xl" v-show="isActive">
        {{text}}
    </div>
</template>

<script>
    export default{
        props:["show", "text"],
        data() {
            return {
                isActive: !!this.show,
                activeTimeout: {}
            };
        },
        mounted() {
            this.setTimeout();
        },
        //Observer Pattern: watcher observes newVal and updates DOM when newVal is changed 
        watch: {
            show(newVal) {
                this.isActive = !!newVal;
            },
            isActive(newVal) {
                if(this.show !== !!newVal){
                    this.$emit("hide-toast", newVal);
                }
                this.setTimeout();
            }
        },
        methods: {
            setTimeout() {
                clearTimeout(this.activeTimeout);
                if(this.isActive) {
                    this.activeTimeout = setTimeout(() => {
                        this.isActive = false
                    }, 2000)
              }
            }
         }
    }
</script>


<style scoped>
    .alert{
        transform: translateX(-50%);
    }
</style>
