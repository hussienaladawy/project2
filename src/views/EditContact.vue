<template>
  <div class="container mt-3">
<div class="row">
  <div class="col">
    <p class="h3 text-success fw-bold">Edit contact</p>
    <p class="fst-italic">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Praesentium quam repudiandae animi consequuntur esse nobis placeat temporibus? Molestiae debitis officia saepe nesciunt, cupiditate nulla dolorem qui reprehenderit non, fugiat eum!</p>
  </div>
</div>
  </div>
  <div class="container">
<div class="row">
   <div class="col-md-4">
<form @submit.prevent="updateSubmit()">
  <div class="mb-2">
      <input type="text" class="form-control" placeholder="Name" v-model="contact.name">
  </div>
  <div class="mb-2">
    <input type="text" class="form-control" placeholder="Photo url" v-model="contact.photo">
</div>
<div class="mb-2">
  <input type="Email" class="form-control" placeholder="Email" v-model="contact.email">
  </div>
  <div class="mb-2">
<input type="number" class="form-control" placeholder="Mobil" v-model="contact.mobile">
  </div>
  <div class="mb-2">
<input type="text" class="form-control" placeholder="Company" v-model="contact.company">
  </div>
  <div class="mb-2">
<input type="text" class="form-control" placeholder="Title" v-model="contact.title">
  </div>
  <div class="mb-2">
  <select v-model="contact.groupId" class="form-control" v-if="groups.length" >
<option value="">select Group</option>
<option  :value="group.id" v-for="group of groups" :key="group.id">{{group.name}}</option>
  </select>
  </div>
  <div class="mb-2">
<input type="submit" class="btn btn-success" value="update">
  </div>
</form>
   </div>
   <div class="col-md-4">
<img :src="contact.photo" alt="" class="contact-img">
   </div>
</div>
  </div>
  <!-- <pre>{{contact}}</pre> -->
</template>
    

<script>
import { ContactService } from '@/services/ContactService';
export default {
  name: "EditContact",

  data: function () {
    return {
      contactId: this.$route.params.contactId,
      loading: false,
      contact: {},
      errorMessage: null,
      groups:[]
    }
  },
  created: async function () {
    try {
      this.loading = true;
      let response = await ContactService.getContact(this.contactId);
      let groupResponse = await ContactService.getAllGroups();
      this.contact = response.data;
      this.groups = groupResponse.data;
      this.loading = false;

    }
    catch (error) {
      this.errorMessage = error;
      this.loading = false;
    }
  },
  methods: {
    updateSubmit: async function () {
      try {
        let response = await ContactService.updateContact(this.contact,this.contactId);
        if (response) {
          return this.$router.push('/');
        }
        else {
          return this.$router.push(`/contacts/edit/${this.contactId}`)
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