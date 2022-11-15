<template>
  <div class="container mt-3">
    <div class="row">
      <div class="col">
        <p class="h3 text-succss fw-bold">Add Contact</p>
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

  <div class="container mt-3">
    <div class="row">
      <div class="col-md-3">
        <form @submit.prevent="submitCreate()">
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
            <select v-model="contact.groupId" v-if="groups.length > 0" class="form-control">
              <option value="">Select Group</option>
              <option v-for="group of groups" :key="group.id" :value="group.id">{{ group.name }}</option>
            </select>
          </div>
          <div class="mb-2">
            <input type="submit" value="Create" class="btn btn-success w-25">
          </div>
        </form>
      </div>
      <div class="col-md-4">
        <img :src="contact.photo" class="contact-img">
      </div>
    </div>
  </div>
</template>

<script>
import { ContactService } from '../services/ContactService';

export default {
  name: "AddContact",
  data: function () {
    return {
      contact: {
        name: '',
        photo: '',
        email: '',
        mobile: '',
        company: '',
        title: '',
        groupId: ''
      },
      groups: [],
      errorMessage: ''
    }
  },
  created: async function () {
    try {
      let response = await this.getAllGroups()

      this.groups = response.data

    } catch (error) {
      this.errorMessage = error
    }
  },
  methods: {
    getAllGroups: async function () {
      return await ContactService.getAllGroups()
    },

    submitCreate: async function () {
      try {
        let response = await ContactService.createContact(this.contact)
        if (response) {
          return this.$router.push('/')
        }else{
          return this.$router.push('/contacts/add')
        }
      } catch (error) {
        this.errorMessage = error
        console.log(error)
      }
    }
  }
}
</script>

<style scoped>

</style>