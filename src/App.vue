<template>
  <div>
      <h1>тестовое приложение </h1>
      <div class="form-wrapper">
          <MyForm 
          @findOne ="postData"
          />
      </div>
      <div>
        <div class="load" v-if="isObjectLoading">
          загружаем оъект
        </div>
        <div v-if="reqDataObj">
          <h2>найденный объект</h2>
          <div >{{ reqDataObj }}</div>
        </div>
        <p v-else> здесь будет показан искомый обьект</p>
      </div>
      <div v-if="!isLoading">
        <h2>Список обьектов</h2>
        <div v-for="obj in arrObj" :key="obj.id">
          <p>{{ obj.email }}</p>
          <p>{{ obj.number }}</p>
        </div>
      </div>
      <div class="load"  v-else>идет загрузка...</div>
  </div>
  
</template>
<script>
import axios from 'axios';
import { ref } from 'vue';


  export default {

      data(){
          return {
              arrObj:[],
              canceled:false,
              count:ref(0),
              reqDataObj:null,
              dataObj:{},
              isLoading:false,
              isObjectLoading:false,
              longFetch:false
          }
      },

      methods:{
       
  
    async getData(){
              try {
                  this.isLoading = true;
                  const response = await axios.get('http://localhost:5001/api/data')
                  this.arrObj = response.data;
              } catch (error) {
                  console.log(error);
              } finally{
                 this.isLoading = false; 
              }
          },
    async postData(obj){
              try {
                this.dataObj.email = obj.email,
                this.dataObj.number = obj.number
                this.isObjectLoading = true;
                this.count +=1
                const response = await axios.post('http://localhost:5001/api/data', 
                  this.dataObj ,
                  {
                    // signal:AbortSignal.timeout(1000),
                    headers: {
                      'Content-Type': 'application/json'
                    }
                  })
                  
                    this.reqDataObj = response.data;
                    this.count = 0
              }catch(e){
                if (e.name =="CanceledError") {
                  console.log('отмена запроса');
                  this.canceled = true
                }
                alert(e.response.data)
              }finally{
                 this.isObjectLoading = false; 
              }
          }
      },

      mounted(){
          this.getData() 
      },

      watch:{
        count(){
          console.log('count');
          if(this.canceled){
            console.log('x', this.count)
            this.canceled = false
          }
        }
      }

      
      
  }
  
</script>
<style scoped>
*{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
.form-wrapper{
  display: flex;
  justify-content: space-between;

}
.load{
  color: red;
  font-size: 30px;
  font-weight: 700;
  margin: 0 auto;
}
</style>
