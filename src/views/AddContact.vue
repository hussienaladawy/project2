<template>
<div class="container mt-3">
<div class="row">
  <div class="col">
    <p class="h3 text-success fw-bold">Add contact</p>
    <p class="fst-italic">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Praesentium quam repudiandae animi consequuntur esse nobis placeat temporibus? Molestiae debitis officia saepe nesciunt, cupiditate nulla dolorem qui reprehenderit non, fugiat eum!</p>
  </div>
</div>
</div>

<div class="container mt-3">
<div class="row">
 <div class="col-md-4">
<form @submit.prevent="submitCreate()">
  <div class="mb-2">
      <input required v-model="contact.name" type="text" class="form-control" placeholder="Name">
  </div>
  <div class="mb-2">
    <input v-model="contact.photo" type="text" class="form-control" placeholder="Photo url">
</div>
<div class="mb-2">
  <input v-model="contact.email" type="Email" class="form-control" placeholder="Email">
</div>
<div class="mb-2">
<input v-model="contact.mobile" type="number" class="form-control" placeholder="Mobil">
</div>
<div class="mb-2">
<input v-model="contact.company" type="text" class="form-control" placeholder="Company">
</div>
<div class="mb-2">
<input v-model="contact.title" type="text" class="form-control" placeholder="Title">
</div>
<div class="mb-2">
<select v-model="contact.groupId" class="form-control" v-if="groups.length> 0 " >
<option value="">select Group</option>
<option :value="group.id" v-for="group of groups" :key="group.id">{{group.name}}</option>
</select>
</div>
<div class="mb-2">
<input type="submit" class="btn btn-success" value="Create">
</div>
</form>
 </div>
 <div class="col-md-4">
<img :src="contact.photo" alt="" class="contact-img">
 </div>
</div>
</div>
<pre>{{contact}}</pre>
</template>

<script>
import { ContactService } from '@/services/ContactService'
export default {

  name: "AddContact",
  data: function () {
    return {
      contact: {
        name: "",
        company: "",
        email: "",
        mobile: "",
        photo: "",
        groupId: "",
        title: ""

      },
      groups: []
    }
  },
  created: async function () {
    try {
      let response = await ContactService.getAllGroups();
      this.groups = response.data;
    }
    catch (error) {
      console.log(error);
    
   }

  },
  methods: {
    submitCreate: async function () {
      try {
        let response = await ContactService.createContact(this.contact);
        if (response) {
          return this.$router.push('/');
        }
        else {
          return this.$router.push('/contacts/add');
        }
      } 
      catch (error) {
        console.log(error);
     }
    }
  }
}

</script>

<style>

</style>