<template>
    <div>
        <h1> Get All Users </h1>
        <div>
            <h2>จำนวนผู้ใช้ {{ users.length }}</h2>
        </div>
        <div v-for="i in users" v-blind:key="i.id">
            <div>ชื่อ {{ i.name }}</div>
            <div>นามสกุล {{ i.lastname }}</div>
            <div>status {{ i.status }}</div>
            <div>type {{ i.type }}</div>
            <div><button v-on:click="navigateTo('/user/' + i.id)">ดูข้อมูลผู้ใช้</button></div>
            <div><button @click="navigateTo('/user/edit/' + i.id)">edit user</button></div>
            <div><button @click="deleteUser(i)">delete user</button></div>
            <hr>
        </div>
    </div>
</template>

<script>
import UsersService from '@/services/UsersService'

export default {
  data() {
    return {
      users: []
    };
  },
  methods:{
    navigateTo(route){
      this.$router.push(route)
    },
    async deleteUser(user){
      let result = confirm("Want of delete?")
      if (result){
        try{
          await UsersService.delete(user)
          this.refreshData()
        }catch(err){
          console.log(err)
        }
      }
    }
  },
  async created() {
    try{
      this.users = (await UsersService.index()).data;
    }catch (err){
      console.log(err)
    }
    
  }
};
</script>

<style scoped></style>