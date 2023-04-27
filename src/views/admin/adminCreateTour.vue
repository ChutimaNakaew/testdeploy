<template>
    <div class="container ">
      <section class="section" id="app">
        <div class="d-flex justify-content-center">
            
          <div class="card" style="width: 60%;">
            <div class="card-body">
              <form action="http://localhost:3000/tours-create" method="POST">
                <div class="mb-3">
                  <label  class="form-label"
                    >ชื่อทัวร์</label
                  >
                  <input type="text" name="name" class="form-control" />
                </div>
                <div class="mb-3">
                  <label  class="form-label"
                    >จังหวัด</label
                  >
                  <input type="text" name="province" class="form-control" />
                </div>
                <div class="mb-3">
                  <label  class="form-label"
                    >ราคา</label
                  >
                  <input type="text" name="price" class="form-control" />
                </div>
                <div class="mb-3">
                  <label  class="form-label"
                    >รายละเอียด</label
                  >
                  <input type="text" name="detail" class="form-control" />
                </div>
                <div class="mb-3">
                  <label  class="form-label"
                    >ตารางการเดินทาง</label
                  >
                  <input type="text" name="schedule" class="form-control" />
                </div>
  
                <label  class="form-label"
                  >วันที่เดินทาง</label
                >
                <div class="mb-3">
                  <label for="example-datepicker">ครั้งที่1</label>
                  <b-form-datepicker
                    id="example-datepicker"
                    class="mb-2"
                    locale="en"
                    name="date_first"
                  ></b-form-datepicker>
                </div>
                <div class="mb-3">
                  <label for="example-datepicker">ครั้งที่2</label>
                  <b-form-datepicker
                    name="date_second"
                    class="mb-2"
                    locale="en"
                  ></b-form-datepicker>
                </div>
                <div class="mb-3">
                  <label class="form-label"
                    >ภาพปก</label
                  >
                  <input type="text" name="img" class="form-control" />
                </div>
                <div class="mb-3">
                  <label  class="form-label"
                    >ภาพรายละเอียด</label
                  >
                  <input type="text" name="detail_img" class="form-control" />
                </div>
                <div class="mb-3">
                  <label  class="form-label"
                    >จำนวนผู้เดินทาง</label
                  >
                  <input type="text" name="max_tourist" class="form-control" />
                </div>
                
            
                <div class="d-grid gap-2 d-md-flex justify-content-md-center">
                  <router-link class="btn btn-secondary " :to="`/admin/manage`">ยกเลิก</router-link>
                  <button class="btn btn-info" type="submit" >สร้างทัวร์</button>
                </div>
              </form>
            </div>
          </div>
        </div>
      </section>
    </div>
  </template>
  <script>
  import axios from "axios";
  // @ is an alias to /src
  export default {
    name: "Create",
    data() {
      return {
        details: {},
        value: "",
        value2:""
        // like: this.$route.body.like,
      };
    },
    mounted() {
      this.getToursDetail(this.$route.params.name);
    },
    methods: {
      getToursDetail(tourName) {
        axios
          .get(`http://localhost:3000/create/${tourName}`)
          .then(response => {
            this.details = response.data;
          })
          .catch(error => {
            this.error = error.response.data.message;
          });
      }
    }
  };
  </script>
  