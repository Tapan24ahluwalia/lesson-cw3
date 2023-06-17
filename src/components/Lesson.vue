
<template>
    <div style="text-align: center;">
       <h2 style="color: black;">Welcome to Lessons!</h2>
       <div id="search">
         <input type="text" v-model="getinput" placeholder="search items..." @input="handleSearch()">
         <!-- <button @click="handleSearch">search</button> -->
       </div>
    </div>
    <div  class="lesson-container">
        <div v-for="item in this.lessons" :key="item._id"  class="lessons" >
           <LessonComp :data="item" @addId="addId($event)" :pageNo="this.pageNo" :lspaces="this.lspaces" /> 
        </div>
    </div>
</template>

<script>
    
import LessonComp from './LessonComp.vue'
    import { toRaw } from 'vue'
    export default {
     name:"Lesson",
     props:['msg','lessons','pageNo','lspaces','sortby','orderby'],
     
     emits:['aId','filter'],
     components:{LessonComp},
     data(){
        return{
          getinput:"",
         
        }
     },
     methods:{
        addId:function(id){
           this.$emit('aId',id)
        },
        handleSearch(){
         //console.log(this.getinput)
         fetch(`http://backend-env.eba-jqbtuzpq.eu-west-2.elasticbeanstalk.com/api/lessons?q=${this.getinput}`,{
               method: "GET", 
               mode: "cors", 
               cache: "no-cache", 
               credentials: "same-origin", 
               headers: {
                  "Content-Type": "application/json",
                  },
               })
               .then(res=>res.json())
               .then(data=>{
                  //console.log(data)
                  this.$emit('filter',data)
               })
               .catch(err=>console.log(err)) 
        },
        
     },
}
      
</script>

<style lang="scss" scoped>

</style>