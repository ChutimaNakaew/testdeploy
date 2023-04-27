<template>
  <div id="app">
    <div class="container-fluid my-3 font">
      <h1>{{ details.name }}</h1>
      <hr />
      <img :src="details.detail_img" width="400" height="200" class="img" />
      <div class="card shadow-sm p-1 my-3 bg-body rounded">
        <div class="card-body">
          <div class="detail">
            <h4>รายละเอียด</h4>
            <p>{{ details.detail }}</p>
          </div>
        </div>
      </div>
      <div class="card shadow-sm p-1 my-3 bg-body rounded">
        <div class="card-body">
          <div class="tourTable">
            <h4>ตารางการเดินทาง</h4>
            <p>{{ details.schedule }}</p>
          </div>
        </div>
      </div>
      <div class="row justify-content-end">
        <router-link
          class="btn btn-primary col-1 mx-3"
          :to="`/tour/${details.name}/form`"
          >จองทัวร์</router-link
        >
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
// @ is an alias to /src
export default {
  name: "Detail",
  data() {
    return {
      search: "",
      details: {},
      // like: this.$route.body.like,
    };
  },
  mounted() {
    this.getToursDetail(this.$route.params.name);
  },
  methods: {
    getToursDetail(tourName) {
      axios
        .get(`http://localhost:3000/tour/${tourName}`)
        .then((response) => {
          this.details = response.data;
        })
        .catch((error) => {
          this.error = error.response.data.message;
        });
    },
  },
};
</script>
<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Kanit:wght@300&display=swap');
.font {
  font-family: "Kanit", sans-serif;
}
.img {
  object-fit: cover;
  display: block;
  margin: auto;
}
</style>