<template>
  <div class="main-wrapper">
    <div v-for="data in details" v-bind:key="data.id" class="card-wrapper">
      <img :src="data.path" class="img-contain" />
      <h1 class="name"><mark>Name:</mark> {{ data.name }}</h1>
      <p><mark>Rating:</mark> 10/{{ data.ratings }}</p>
      <div class="d-flex justify-content-center">
        <b-button
          v-b-modal.edit-modal
          @click="elementId(data)"
          type="button"
          class=" btn-warning btn-adjust"
          >Edit</b-button
        >
        <b-button
          @click="deleteList(data)"
          class=" btn-danger btn-adjust mt-2"
          >Delete</b-button
        >
      </div>
      <a :href="data.path" download
        ><b-button
          id="downloadImg"
          variant="outline-success"
          class="mt-2"
        >
          Download</b-button
        ></a
      >
      <b-modal
        id="edit-modal"
        title="Edit Details"
        size="lg"
        v-if="data.id == clickedId"
        hide-footer
        centered
      >
        <b-form>
          <img
            class="img-wrapper"
            v-if="toggleImg2 && data.id == clickedId"
            :src="data.path"
          />
          <PhotoCapture
            v-if="toggleImg1 && data.id == clickedId"
            v-model="img"
          />
          <div class="step-container">
            <h1 class="click-step" v-if="toggleImg1 && data.id == clickedId">
              Just click capture and then ok and last save changes
            </h1>
          </div>
          <div class="d-flex justify-content-center">
            <b-button
              v-if="toggleImg2 && data.id == clickedId"
              class=" btn-dark upload-btn"
              @click="retakeImg(data)"
              >Retake</b-button
            >
          </div>
          <b-form-input
            v-model="text"
            v-bind:placeholder="data.name"
          ></b-form-input>
          <b-form-rating
            id="rating-10"
            variant="warning"
            v-model="rating"
            stars="10"
          ></b-form-rating>
          <div class="d-flex justify-content-center">
            <b-button
              type="submit"
              @click="onSubmit(data)"
              variant="primary"
              class=" btn-success upload-btn"
              >Save Changes</b-button
            >
          </div>
        </b-form>
      </b-modal>
    </div>
  </div>
</template>

<script>
import "../assets/scss/main.scss";
import { PhotoCapture } from "vue-media-recorder";
export default {
  data() {
    return {
      details: [],
      clickedId: null,
      rating: "",
      img: "",
      toggleImg1: false,
      toggleImg2: true,
      text: "",
    };
  },
  components: {
    PhotoCapture,
  },
  methods: {
    retakeImg() {
      this.toggleImg1 = true;
      this.toggleImg2 = false;
    },
    deleteList(data) {
      const dataIndex = this.details.indexOf(data);
      this.details.splice(dataIndex, 1);
      localStorage.setItem("details", JSON.stringify(this.details));
    },
    elementId(data) {
      const dataIndex = this.details.indexOf(data);
      this.clickedId = this.details[dataIndex].id;
    },
    onSubmit(data) {
      let name = data.name;
      let path = data.path;
      let ratings = data.ratings;
      if (this.img == "" && this.text == "") {
        data.ratings = this.rating;
        data.name = name;
        data.path = path;
      } else if (this.text == "" && this.rating == "") {
        data.path = this.img;
        data.name = name;
        data.ratings = ratings;
      } else if (this.img == "" && this.rating == "") {
        data.name = this.text;
        data.path = path;
        data.ratings = ratings;
      }
      localStorage.details = JSON.stringify(this.details);
    },
  },
  mounted() {
    this.details = JSON.parse(localStorage.details);
  },
};
</script>

<style scoped>
.main-wrapper {
  display: grid;
  grid-template-columns: repeat(auto-fill, 300px);
  gap: 30px;
  position: relative;
  justify-content: center;
  margin-bottom: 100px;
  z-index: 1;
  top: 51px;
  padding: 43px;
  margin-left: 49px;
  margin-right: 49px;
  border: 2px solid #33a2b8;
  border-radius: 30px;
  box-shadow: 0px 0px 50px -2px rgb(0 0 0 / 29%);
}
@media screen and (max-width: 420px) {
  .main-wrapper {
    grid-template-columns: repeat(auto-fill, 200px);
  }
}
.card-wrapper {
  text-align: center;
  -webkit-box-shadow: 0px 0px 13px -2px rgba(0, 0, 0, 0.29);
  -moz-box-shadow: 0px 0px 13px -2px rgba(0, 0, 0, 0.29);
  box-shadow: 0px 0px 13px -2px rgba(0, 0, 0, 0.29);
  padding-bottom: 8px;
  display: grid;
  animation-name: power;
  width: 100%;
  animation-duration: 1s;
  background-color: seashell;
  border-radius: 30px;
}
.img-wrapper {
  height: 500px;
  width: 100%;
  object-fit: cover;
}
.click-step {
  font-size: 25px;
  background-color: black;
  color: white;
  display: inline-block;
  padding: 10px;
  border: 1px dashed red;
  border-radius: 29px;
  -webkit-text-emphasis-style: triangle;
}
.step-container {
  text-align-last: center;
}
.img-contain {
  height: 300px;
  width: 100%;
  object-fit: cover;
  border-radius: 30px;
}
.name {
  word-break: break-all;
  font-size: 15px;
  width: 100%;
  font-weight: bold;
  margin-top: 10px;
}
.btn-adjust {
  width: 78px;
  justify-self: center;
}
mark {
  background-color: yellow;
  font-weight: bold;
}
@keyframes power {
  0% {
    transform: scale(0.5, 0.5);
  }
  100% {
    transform: scale();
  }
}
</style>
