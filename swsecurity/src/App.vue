<template>
  <form @submit.prevent="postData">
    <input type="text" v-model="name">
    <button type="submit">Submit</button>
    <div>
      <p v-if="response">{{ response }}</p>
  </div>  </form>
</template>

<script>
import axios from "axios";
import { PublicClientApplication } from "@azure/msal-browser";

export default {
  name: "MyComponent",
  data() {
    return {
      response: null
    };
  },
  methods: {
    async postData() {
      axios.defaults.withCredentials = true; // Enable cookies
      const msalConfig = {
        auth: {
          clientId: "c91bcf0f-95db-4d61-996e-5e8a717a6839",
          authority: "https://login.microsoftonline.com/f7b226f1-9f32-4abf-8727-ed765898b168",
          redirectUri: "https://salmon-wave-0cfcc961e.2.azurestaticapps.net" // The URL of your Vue.js application
        }
      };

      const msalInstance = new PublicClientApplication(msalConfig);

      const loginRequest = {
        scopes: ["api://c91bcf0f-95db-4d61-996e-5e8a717a6839/staticwebapp2"]
      };

      try {
        const authResult = await msalInstance.loginPopup(loginRequest);
        const accessToken = authResult.accessToken;

        const config = {
          headers: {
            Authorization: `Bearer ${accessToken}`,
            "Content-Type": "application/json"
          }
        };

        const data = {
          name: "josue"
        };

        const response = await axios.post("https://fa-testing-functionsecurity.azurewebsites.net/api/httptrigger_security", JSON.stringify(data), config);
        this.response = response.data;
      } catch (error) {
        console.log(error);
      }
    }
  }
};

/*export default {
  data() {
    return {
      responseData: ''
    }
  },
  methods: {
    postData() {
      axios.post('https://fa-testing-functionsecurity.azurewebsites.net/api/httptrigger_security', {
        name: this.name
      })
        .then(response => {
          // Handle the API response here
          this.responseData = response.data;
          console.log(response.data);
        })
        .catch(error => {
          // Handle any errors here
          console.error(error);
        });
    }
  }
}*/
</script>