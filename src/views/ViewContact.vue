<template>
  <div class="container mt-3">
    <div class="row">
      <div class="col">
        <p class="h3 text-success fw-bold">
          View Contact
        </p>

        <p class="fst-italic">
          Lorem ipsum dolor sit amet consectetur, adipisicing elit. Animi, soluta a. Nihil cumque at laboriosam quis
          dolores perferendis saepe asperiores. Similique dolore vel est! Neque iusto repellat ipsa perferendis
          veritatis!
        </p>
      </div>
    </div>
  </div>


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
  <div v-if="!loading && isDone()" class="mt-5">
    <div class="container">
      <div class="row">
        <div class="col text-center">
          <p class="h3 text-danger fw-bold">{{ errorMessage }}</p>
        </div>
      </div>
    </div>
  </div>


  <div class="container" v-if="!loading && !errorMessage">
    <div class="row aling'align-items-center">
      <div class="col-md-4">
        <img :src="contact.photo" alt="imagen de usuario" class="contact-img-big">
      </div>

      <div class="col-md-6">
        <ul class="list-group">
          <li class="list-group-item">Name: <span class="fw-bold">{{ contact.name }}</span></li>
          <li class="list-group-item">Email: <span class="fw-bold">{{ contact.email }}</span></li>
          <li class="list-group-item">Mobile: <span class="fw-bold">{{ contact.mobile }}</span></li>
          <li class="list-group-item">Company: <span class="fw-bold">{{ contact.company }}</span></li>
          <li class="list-group-item">Title: <span class="fw-bold">{{ contact.title }}</span></li>
          <li class="list-group-item">Group: <span class="fw-bold">{{ group.name }}</span></li>
        </ul>
      </div>
    </div>

    <div class="row mt-2">
      <div class="col">
        <router-link to="/" class="btn btn-danger">
          <i class="fa fa-arrow-alt-circle-left"></i>
          Back
        </router-link>
      </div>
    </div>
  </div>


</template>
<script>
import { ContactService } from '../services/ContactService'
import SpinnerItem from '../components/Spinner.vue'
export default {
  name: "ViewContact",
  components: { SpinnerItem },
  data: function () {
    return {
      contactId: this.$route.params.contactId,
      loading: false,
      contact: {},
      group: {},
      errorMessage: null
    }
  },
  created: async function () {
    try {
      this.loading = true
      let responseContact = await ContactService.getContact(this.contactId)
      this.contact = responseContact.data

      let responseGroup = await ContactService.getGroup(responseContact.data)

      this.group = responseGroup.data



      this.loading = false

    } catch (error) {
      this.loading = false
      this.errorMessage = console.error();
      console.log(error)
    }

  },
  methods: {
    isDone: function () {
      return Object.keys(this.contact).length > 0 && Object.keys(this.group).length > 0
    }
  }
}
</script>

<style scoped>

</style>