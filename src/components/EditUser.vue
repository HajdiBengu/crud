<script>
import axios from "axios";
export default {
  data() {
    return {
      loading: true,
      checkbox: false,
      fname: "",
      address: "",
      username: "",
      city: "",
      email: "",
      zip: "",
      phone: "",
      lat: "",
      lon: "",
      nameRules: [
        (v) =>
          !v ||
          /^([\w]{2,})+\s+([\w\s]{2,})+$/i.test(v) ||
          "Please enter your full name",
        (v) => {
          if (v) return true;
          return "Please enter your full name";
        },
      ],
      emailRules: [
        (v) =>
          !v ||
          /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(v) ||
          "E-mail must be valid",
        (v) => {
          if (v) return true;
          return "You must enter a valid email.";
        },
      ],
      zipRules: [
        (v) =>
          !v ||
          /(^\d{5}$)|(^\d{5}-\d{4}$)/.test(v) ||
          "Zipcode must be 5 digits.",
      ],
    };
  },
  props: ["id"],
  emits: ["close"],
  async mounted() {
    try {
      const res = await axios.get(
        `https://jsonplaceholder.typicode.com/users/${this.id}`
      );
      this.fname = res.data.name;
      this.username = res.data.username;
      this.city = res.data.address.city;
      this.email = res.data.email;
      this.zip = res.data.address.zipcode;
      this.phone = res.data.phone;
      this.lat = res.data.address.geo.lat;
      this.lon = res.data.address.geo.lng;
      this.address = `${res.data.address.suite}, ${res.data.address.street}`;
      this.loading = false;
    } catch (error) {
      console.error(error);
    }
  },
  methods: {
    closeModal() {
      this.$emit("close");
    },
    async submit() {
      try {
        const res = await axios.post(
          "https://jsonplaceholder.typicode.com/users",
          {
            name: this.fname,
            username: this.username,
            email: this.email,
            address: {
              street: this.address,
              city: this.city,
              zipcode: this.zip,
              geo: {
                lat: this.lat,
                lng: this.lon,
              },
            },
            phone: this.phone,
          }
        );
        console.log(res.data);
      } catch (error) {
        console.error(error);
      }
    },
  },
};
</script>

<template>
  <div class="backdrop" @click.self="closeModal">
    <div class="modal">
      <v-form class="px-3">
        <v-checkbox v-model="checkbox" label="Use Google Location"></v-checkbox>
        <v-text-field label="Full Name" v-model="fname" :rules="nameRules">
        </v-text-field>
        <v-text-field label="Address" v-model="address"> </v-text-field>
        <v-text-field label="Username" v-model="username"> </v-text-field>
        <v-text-field label="City" v-model="city"> </v-text-field>
        <v-text-field label="Email" v-model="email" :rules="emailRules">
        </v-text-field>
        <v-text-field label="Zip Code" v-model="zip" :rules="zipRules">
        </v-text-field>
        <v-text-field label="Phone Nr" v-model="phone"> </v-text-field>
        <div v-if="checkbox">
          <v-text-field label="Latitude" v-model="lat"> </v-text-field>
          <v-text-field label="Longitude" v-model="lon"> </v-text-field>
        </div>

        <v-btn flat @click.prevent="submit">Save</v-btn>
      </v-form>
    </div>
  </div>
</template>

<style>
.modal {
  width: 590px;
  padding: 20px;
  margin: auto;
  background: white;
  border-radius: 5px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.backdrop {
  top: 0;
  position: fixed;
  background: rgba(0, 0, 0, 0.5);
  width: 100%;
  height: 100%;
}
</style>
