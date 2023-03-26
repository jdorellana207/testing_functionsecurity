<template>
  <form @submit.prevent="postData">
    <input type="text" v-model="name">
    <button type="submit">Submit</button>
  </form>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      name: ''
    }
  },
  methods: {
    postData() {
      axios.post('https://fa-testing-functionsecurity.azurewebsites.net/api/httptrigger_security', {
        name: this.name
      })
        .then(response => {
          // Handle the API response here
          console.log(response.data);
        })
        .catch(error => {
          // Handle any errors here
          console.error(error);
        });
    }
  }
}
</script>