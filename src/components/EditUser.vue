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

      emailRules: [
        (v) =>
          !v ||
          /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,4})+$/.test(v) ||
          "E-mail must be valid",
        (v) => {
          if (v) return true;
          return "You must enter a valid email.";
        },
      ],
      zipRules: [
        (v) => {
          if (v.length === 5 || v.length === 10) return true;
          return "Please enter a valid zipcode";
        },
      ],
      coordRules: [
        (v) =>
          !v ||
          /^-?([0-9]{1,2}|1[0-7][0-9]|180)(\.[0-9]{1,10})$/.test(v) ||
          "Not valid",
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

    async showAddress() {
      try {
        const res = await axios.get(
          `https://maps.googleapis.com/maps/api/geocode/json?latlng=${this.lat},${this.lon}&location_type=ROOFTOP&result_type=street_address&key=AIzaSyAu3RlWLLRjAwI2KDOXDuiUP0rZ8kRKPgE`
        );
        this.address = res.data.results[0].formatted_address;
      } catch (error) {
        console.error(error);
      }
    },

    async submit() {
      try {
        const res = await axios.put(
          `https://jsonplaceholder.typicode.com/users/${this.id}`,
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
      <v-form
        class="d-flex flex-wrap"
        validate-on="input submit"
        @submit.prevent="submit"
      >
        <v-text-field label="Full Name" class="pa-2 w-50" v-model="fname" />

        <v-text-field
          label="Address"
          class="pa-2 w-50"
          v-model="address"
          id="autocomplete"
        />

        <v-text-field label="Username" class="pa-2 w-50" v-model="username" />

        <v-text-field label="City" class="pa-2 w-50" v-model="city" />

        <v-text-field
          label="Email"
          class="pa-2 w-50"
          v-model="email"
          :rules="emailRules"
        />

        <v-text-field
          label="Zip Code"
          class="pa-2 w-50"
          v-model="zip"
          :rules="zipRules"
        />

        <v-text-field label="Phone Nr" class="pa-2 w-50" v-model="phone" />

        <div class="pa-2 w-50 d-flex flex-row flex-wrap">
          <v-text-field
            label="Latitude"
            class="w-50"
            v-model="lat"
            :rules="coordRules"
          />
          <v-text-field
            label="Longitude"
            class="w-50"
            v-model="lon"
            :rules="coordRules"
          />
        </div>

        <v-btn
          class="pa-2 w-50"
          flat
          type="submit"
          text="Show Address"
          @click.prevent="showAddress"
        />
        <v-btn flat class="mt-auto pa-2 w-50" type="submit" text="Save" />
      </v-form>
    </div>
  </div>
</template>

<style>
.modal {
  width: 690px;
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
