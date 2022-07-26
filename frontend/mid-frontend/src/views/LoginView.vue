<template>
       <div class="row mt-3">
          <div class="col-md-4 offset-md-4">
               <div class="card">
                    <div class="card-header bg-dark text-center text-light">
                         <h4> User Login</h4>
                    </div>
                    <form @submit.prevent="handleSubmit">
                          <div class="card-body bg-light bg-opacity-70">
                               <div class="mb-3">
                                        <label for="email">Email</label>
                                        <input type="email" id="email" class="form-control" v-model="user.email" required>
                                </div>
                                <div class="mb-3">
                                        <label for="password">Password</label>
                                        <input type="password" id="password" class="form-control" v-model="user.password" required>
                                </div>
                                <div class="card-footer d-flex justify-content-center">
                                        <button class=" btn btn-success btn-outline-dark" type="submit" >
                                             Log in 
                                        </button>
                                </div>
                         </div>
                     </form>
               </div>
          </div>     
     </div>
     
</template>
<script>
import { ref } from '@vue/reactivity'
import { useAuthStore } from '../stores/auth'
import { useRouter } from 'vue-router'

export default {
     setup() {
        const authStore = useAuthStore()
        const router = useRouter()

        const user = ref ({ email:"",  password:"" })
          async function handleSubmit(){
              await fetch('http://localhost:8000/api/login', {
               method: 'post',
               headers: {
                    "Accept":"Application/json",
                    "Content-Type":"application/json"
               },
               body: JSON.stringify(user.value)
              }).then(response=>response.json())
              .then(data=>{
               if(data.status=="success"){
                    authStore.saveAuth(data.user, data.token)
                    router.push('/profile')
                 }else(
                    alert(data.message)
                 )
              })
          }
         return {
               user,
               handleSubmit
          }
     },
}
</script>
<style scoped>
 .body {
        background-image: url('images/bg.jpg');
        background-repeat: no-repeat;
        background-attachment: fixed;
        background-size: cover;
        }
</style>