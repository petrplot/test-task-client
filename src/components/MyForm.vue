<template>
    <form 
        class="form" 
        @submit.prevent  
    >
        <h2>Найти данные</h2>
        <MyInput
            v-model="dataObj.email" 
            type="text"
            placeholder="email"
        />
        <MyInput
            v-model="dataObj.number" 
            type="text"
            v-imask="mask"
            @complete="onComplete"
        />
            <MyButton 
                @click="validVAlue"
            >
                получить
            </MyButton>    
    </form>
</template>

<script>

import { IMaskDirective } from 'vue-imask';
export default {
    name:"MyForm",
    data() {
        return {
            dataObj: {
                email: "",
                number: null,
            },
            mask: {
                mask: "00-00-00",
                lazy: false
            },
      }
    } ,
    methods:{

    validVAlue(){
        if(this.dataObj.number
        &&
        this.dataObj.email 
        ){ 
            this.dataObj.number = this.dataObj.number.replace(/(-|_)/g,'')
            this.sendEndClearForm()
        }else{
          alert('введите корректные данные')  
        }
         this.dataObj = {
            email:'',
            number:''
        }
        
        
    },

    sendEndClearForm(){
        this.$emit('findOne',this.dataObj)
    },

      onComplete (e) {
        const maskRef = e.detail;
        this.dataObj.number = maskRef.unmaskedValue
        
      }

    },
    directives: {
      imask: IMaskDirective
    }

}

</script>

<style scoped>
.form{
    display: flex;
    flex-direction: column;
    
}
</style>