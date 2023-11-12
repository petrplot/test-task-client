<template>
    <input class="inp" :value="modelValue" @input="updateInput">
</template>

<script>


    export default {
        name:'MyInput',
        props:{
            modelValue:[String, Number],
            
        },
        methods:{
            updateInput(e){
                this.$emit('update:modelValue', e.target.value)
                
            }
        }
    }
</script>

<style scoped>
.inp{
    width: 50%;
    border: 2px solid green;
    padding: 10px 15px;
    margin-top: 10px;
}
</style>