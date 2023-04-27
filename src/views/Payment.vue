<template>
  <div id="app">
    <div class="container-fluid font">
      <h3>&nbsp;</h3>
      <div class="row">
        <div class="col-12 d-flex align-items-center flex-column">
          <div class="card card-w">
            <div class="card-body">
              <div class="row">
                <div class="col-12">
                  <h4>ช่องทางการชำระเงิน</h4>
                </div>
              </div>
              <div class="row mt-3">
                <div
                  class="col-6 p-2 d-inline"
                  v-for="pay in pays"
                  :key="pay.id"
                >
                  <div class="row p-2">
                    <div class="col-4">
                      <img :src="pay.account_img" />
                    </div>
                    <div class="col-8">
                      <p>{{ pay.name }}</p>
                      <div class="row">
                        <div class="col">
                          <p>หมายเลขบัญชี</p>
                        </div>
                        <div class="col">
                          <p>{{ pay.account_number }}</p>
                        </div>
                      </div>
                      <div class="row">
                        <div class="col">
                          <p>ชื่อบัญชี</p>
                        </div>
                        <div class="col">
                          <p>{{ pay.account_name }}</p>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
              <div class="row">
                <div class="col-12 d-flex align-items-center flex-column">
                  <button
                    class="btn btn-primary"
                    data-bs-toggle="modal"
                    data-bs-target="#exampleModal"
                  >
                    ส่งหลักฐานการชำระเงิน
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- Modal -->
      <div
        class="modal fade"
        id="exampleModal"
        tabindex="-1"
        aria-labelledby="exampleModalLabel"
        aria-hidden="true"
      >
        <div class="modal-dialog">
          <div class="modal-content">
            <form @submit.prevent="submit" enctype="multipart/form-data">
              <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLabel">
                  ส่งหลักฐานการชำระเงิน
                </h5>
                <button
                  type="button"
                  class="btn-close"
                  data-bs-dismiss="modal"
                  aria-label="Close"
                ></button>
              </div>
              <div class="modal-body">
                <div class="row">
                  <div class="col">ธนาคารที่ทำการชำระเงิน</div>
                  <div class="col">
                    <select
                      name="bank"
                      v-model="bank"
                      class="form-select form-select-sm"
                      aria-label=".form-select-sm example"
                    >
                      <option value="">Open this select menu</option>
                      <option value="ธนาคารกสิกรไทย">ธนาคารกสิกรไทย</option>
                      <option value="ธนาคารไทยพาณิชย์">ธนาคารไทยพาณิชย์</option>
                      <option value="ธนาคารกรุงไทย">ธนาคารกรุงไทย</option>
                      <option value="ธนาคารกรุงศรีอยุธยา">
                        ธนาคารกรุงศรีอยุธยา
                      </option>
                      <option value="ธนาคารออมสิน">ธนาคารออมสิน</option>
                      <option value="ธนาคารทหารไทย">ธนาคารทหารไทย</option>
                    </select>
                  </div>
                </div>
                <span
                  ><p v-if="err.length && bank == ''">
                    <span
                      v-for="error in err"
                      :key="error.key"
                      style="color:red;"
                    >
                      <p v-if="error === 'กรุณาเลือกธนาคาร'">
                        <span>
                          {{ error }}
                        </span>
                      </p>
                    </span>
                  </p></span
                >
                <!-- <span v-if="err.has('bank')">{{err.first('bank')}} </span>   -->

                <div class="row my-2">
                  <div class="form-group">
                    <label class="form-label">รหัสลูกค้า</label>
                    <input
                      class="form-control"
                      type="text"
                      readonly
                      :value="info.bookingId"
                    />
                    <!-- <select
                      class="form-select"
                      name="bookingId"
                      v-model="form.bookingId"
                    >
                      <option>{{ info.bookingId }}</option>
                    </select> -->
                  </div>
                </div>
                <div class="row my-2">
                  <div class="col-12">
                    <label class="form-label">สลิปการโอนเงิน</label>
                    <input
                      name="slip"
                      accept="image/*"
                      class="form-control form-control-sm"
                      type="file"
                      @change="saveImg"
                    />
                  </div>
                </div>
                <span
                  ><p v-if="err.length && slip == null">
                    <span
                      v-for="error in err"
                      :key="error.key"
                      style="color:red;"
                    >
                      <p v-if="error === 'กรุณาอัพโหลดหลักฐานชำระเงิน'">
                        <span>
                          {{ error }}
                        </span>
                      </p>
                    </span>
                  </p></span
                >
              </div>
              <div class="modal-footer">
                <button
                  type="button"
                  class="btn btn-secondary"
                  data-bs-dismiss="modal"
                >
                  ยกเลิก
                </button>
                <button v-if="err.length >= 0 && (bank == '' || slip == null)"
                  type="submit"
                  class="btn btn-primary"
                >
                  ยืนยัน
                </button>
                <button v-else
                  data-bs-dismiss="modal"
                  type="submit"
                  class="btn btn-primary"
                >
                  ยืนยัน
                </button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
// @ is an alias to /src
export default {
  name: "Payment",
  data() {
    return {
      search: "",
      pays: [],
      info: {},
      bank: "",
      slip: null,
      err: []
    };
  },
  mounted() {
    this.getPayment(this.$route.params.id);
    this.getConfirmDetail(this.$route.params.id);
  },
  methods: {
    getPayment(id) {
      axios
        .get(`http://localhost:3000/form/${id}/payment`)
        .then(response => {
          this.pays = response.data;
        })
        .catch(error => {
          this.error = error.response.data.message;
        });
    },
    getConfirmDetail(id) {
      axios
        .get(`http://localhost:3000/form/${id}/confirm`)
        .then(response => {
          this.info = response.data;
        })
        .catch(error => {
          this.error = error.response.data.message;
        });
    },
    saveImg(e) {
      const slip = e.target.files[0];
      this.createBase64Image(slip);
    },
    createBase64Image(fileObject) {
      const reader = new FileReader();

      reader.onload = e => {
        this.slip = e.target.result;
      };

      reader.readAsDataURL(fileObject);
      console.log(this.slip);
    },
    submit(e) {
      e.preventDefault();
      if (this.bank && this.slip) {
        axios
          .post("http://localhost:3000/user-update", {
            bank: this.bank,
            bookingId: this.info.bookingId,
            slip: this.slip
          })
          .then(response => {
            this.$router.push({
              name: "status",
              params: { id: response.data }
            });
          })
          .catch(error => {
            console.log(error.message);
          });
      }
      this.err = [];

      if (!this.bank) {
        this.err.push("กรุณาเลือกธนาคาร");
      } else if (!this.slip) {
        this.err.push("กรุณาอัพโหลดหลักฐานชำระเงิน");
      }
    }
  },
  computed: {
    errors: function() {
      if (!this.bank) {
        return ["กรุณาเลือกธนาคาร"];
      }
      if (!this.slip) {
        return ["กรุณาอัพโหลดหลักฐานชำระเงิน"];
      } else {
        return [];
      }
    }
  }
};
</script>
<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Kanit:wght@300&display=swap");

.font {
  font-family: "Kanit", sans-serif;
}

.card-w {
  width: 90rem;
}

img {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
}
</style>
