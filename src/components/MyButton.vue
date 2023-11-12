<template>
   <button class="btn">
        <slot></slot>
   </button>
</template>

<script>
    export default {
        name:'MyButton'
    }
</script>

<style scoped>
.btn{
    width: 100px;
    height: 40px;
    background: rgb(0, 17, 255);
    border-radius: 5px;
    margin: 10px;
    border: none;
}
</style>