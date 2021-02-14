<template>
  <div class="main-container">
    <div class="container mt-3 pb-3">
      <b-form @submit="onSubmit">
        <b-button
          v-b-modal.take-pic
          variant="primary"
          class=" btn-success upload-btn mb-2"
          >Take Photo</b-button
        >
        <b-modal id="take-pic" size="lg" modal-ok>
          <template #modal-footer="{}">
            <b-button size="sm" variant="success" @click="toggle">
              Switch
            </b-button>
          </template>
          <div v-show="imgClicked">
            <PhotoCapture v-model="img" />
          </div>
          <div v-show="!imgClicked">
            <img :src="this.img" class="img-wrapper" />
            <h1>Press on X to close Modal</h1>
          </div>
        </b-modal>
        <b-form-input
          required
          v-model="text"
          placeholder="Enter your name"
        ></b-form-input>
        <b-form-rating
        required
          id="rating-10"
          variant="warning"
          v-model="rating"
          stars="10"
        ></b-form-rating>
        <b-button
          type="submit"
          variant="primary"
          class=" btn-success upload-btn"
          >Upload</b-button
        >
      </b-form>
    </div>
  </div>
</template>

<script>
import "../assets/scss/main.scss";
import { PhotoCapture } from "vue-media-recorder";

export default {
  components: {
    PhotoCapture,
  },
  data() {
    return {
      details: [],
      text: "",
      img: "",
      rating: null,
      imgClicked: true,
    };
  },
  watch: {
    img() {
      if (this.imgClicked == true) {
        this.imgClicked = false;
      } else {
        this.imgClicked = true;
      }
    },
  },
  methods: {
    onSubmit(event) {
      this.details.push({
        id: event.timeStamp,
        name: this.text,
        ratings: this.rating,
        path: this.img,
      });
      localStorage.details = JSON.stringify(this.details);
    },
    toggle() {
      if (this.imgClicked == true) {
        this.imgClicked = false;
      } else {
        this.imgClicked = true;
      }
    },
  },
  mounted() {
    this.details = JSON.parse(localStorage.details);
  },
};
</script>

<style scoped>
.upload-btn {
  display: flex;
  margin: auto;
  margin-top: 10px;
}
.container {
  border: 2px solid #33a2b8;
  box-shadow: 0px 0px 13px -2px rgba(0, 0, 0, 0.29);
  border-radius: 30px;
}
h1 {
  font-size: 20px;
  color: #fff;
  background-color: black;
  margin-top: 10px;
  text-align: center;
}
</style>