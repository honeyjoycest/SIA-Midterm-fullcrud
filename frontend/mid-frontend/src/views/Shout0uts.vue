
<template>
          <h2 class="text-light">Shouts Page</h2> 
        <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/font-awesome/4.4.0/css/font-awesome.min.css">
 <div class="card-header text-danger bg-dark text-light text-center ">Shoutouts Info
     </div>
          <div class="card-body bg-info ">
              <div class="card-title">
                   <table class="table bg-light table-striped table-bordered border-dark">
             
                         <thead>
                                   <th> User ID</th>
                                   <th> Shoutout</th>                        
                                   <th> Tags </th>               
                         </thead>
                  <tbody>
                    <tr v-for="shouty in shouts" :key="shouty.id" >
                      <td>{{shouty.user_id}}</td>
                      <td>{{shouty.shoutout}}</td>
                      <td>{{shouty.tags}}</td>
                  </tr>
                  </tbody>
              </table>
           </div>
     </div>
</template>
<script>
import { useAuthStore } from '../stores/auth'
import { useRouter } from 'vue-router'
export default {
      data() {
        return {
            shouts: [],
        }
    },
   
  async  mounted(){
     await fetch('http://localhost:8000/api/shouts',{
          method:'get',
          headers:{
               "Accept":"application/json",
               "Authorization":"Bearer " + this.token
          }
            }).then(res=>res.json())
            .then(data=>this.shouts = data)
            .catch(err=>console.log(err))         
        },
        openShouts(shoutoutId) {
            this.$router.push({
                name: 'shoutout',
                params: {
                    id: shoutoutId
                }
            })
        },
    setup(){
     const { user, token } =  useAuthStore()    
          const router = useRouter()    

          if(token==""){
               router.replace('/login')
          }
          
               return{
                    token,
                    user
         }
     },
}
</script>
