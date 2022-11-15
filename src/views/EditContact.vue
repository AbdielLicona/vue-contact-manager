<template>
  <div class="container mt-3">
    <div class="row">
      <div class="col">
        <p class="h3 text-succss fw-bold">Edit Contact</p>
        <p class="fst-italic">
          Lorem ipsum dolor sit amet, consectetur adipisicing elit. Officiis molestiae distinctio porro quam, nam
          voluptatum eligendi eius vero amet nostrum quod sapiente soluta reprehenderit modi sequi! Vitae ipsum rerum
          atque.
        </p>
      </div>
    </div>
  </div>

  <!-- <pre>
    {{ contact }}
  </pre> -->

  <!-- Spinner -->
  <div v-if="loading">
    <div class="container">
      <div class="row">
        <div class="col">
          <SpinnerItem />
        </div>
      </div>
    </div>
  </div>

  <!-- Error Message -->
  <div v-if="!loading && errorMessage" class="mt-5">
    <div class="container">
      <div class="row">
        <div class="col text-center">
          <p class="h3 text-danger fw-bold">{{ errorMessage }}</p>
        </div>
      </div>
    </div>
  </div>

  <div class="container mt-3">
    <div class="row">
      <div class="col col-md-3">

        <form @submit.prevent="updateSubmit()">
          <div class="mb-2">
            <input v-model="contact.name" type="text" class="form-control" placeholder="Name">
          </div>
          <div class="mb-2">
            <input v-model="contact.photo" type="text" class="form-control" placeholder="Photo URL">
          </div>
          <div class="mb-2">
            <input v-model="contact.email" type="text" class="form-control" placeholder="Email">
          </div>
          <div class="mb-2">
            <input v-model="contact.mobile" type="text" class="form-control" placeholder="Mobile">
          </div>
          <div class="mb-2">
            <input v-model="contact.company" type="text" class="form-control" placeholder="Company">
          </div>
          <div class="mb-2">
            <input v-model="contact.title" type="text" class="form-control" placeholder="Title">
          </div>
          <div class="mb-2">
            <select v-model="contact.groupId" class="form-control" v-if="groups.length > 0">
              <option value="">Select Group</option>
              <option :value="group.id" v-for="group of groups" :key="group.id">{{group.name}}</option>
            </select>
          </div>
          <div class="mb-2">
            <input type="submit" value="Update" class="btn btn-success ">
          </div>
        </form>


      </div>

      <div class=" col col-md-4">
        <img :src="contact.photo" alt="imagen de usuario" class="contact-img">
      </div>
    </div>
  </div>
</template>
<script>

import { ContactService } from "../services/ContactService.js";
import SpinnerItem from "../components/Spinner.vue";



export default {
  name: "EditContact",
  components: { SpinnerItem },
  data: function () {
    return {
      contactId: this.$route.params.contactId,
      loading: false,
      contact: {},
      groups: [],
      errorMessage: null
    }
  },
  created: async function () {
    try {
      this.loading = true
      let response = await ContactService.getContact(this.contactId)
      this.contact = response.data

      let groupsResponse = await ContactService.getAllGroups()
      this.groups = groupsResponse.data


      this.loading = false
    } catch (error) {
      this.errorMessage = error
      this.loading = false
    }
  },
  methods: {
    updateSubmit: async function () {
      try {
        let response = await ContactService.updatedContact(this.contact, this.contactId)

        if (response) {
          return this.$router.push('/')
        }else{
          return this.$router.push(`/contacts/edit/${this.contactId}`)
        }

      } catch (error) {
        console.log(error)
      }
    }
  }
}
</script>

<style scoped>

</style>