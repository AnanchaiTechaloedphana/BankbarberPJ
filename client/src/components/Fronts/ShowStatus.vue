<template>
  <div>
    <main-header navsel="front"></main-header>
    <div class="header">
      <h3><i class="fas fa-tasks"></i> ยืนยัน</h3>
    </div>
    <div class="container-fluid">
      <div class="container">
        <div class="blog-wrapper">
         
   
            <div v-if="user.type == 'admin'">
              <h4><i class="fas fa-clipboard-list"></i> ทำการยืนยัน</h4>
              <hr />
              <br />
              <center>
                <div style="width: 50%">
                  <form v-on:submit.prevent="editBorrow">
                    <div class="col" align="left">
                      <label for="input1">ชื่อแอดมิน</label>
                      <div class="input-group mb-3">
                        <div class="input-group-prepend">
                          <span class="input-group-text" id="basic-addon1"
                            ><i class="fas fa-user-cog"></i
                          ></span>
                        </div>
                        <input
                          required
                          type="tet"
                          class="form-control"
                          placeholder="Name"
                          v-model="user.name"
                          disabled
                        />
                      </div>
                    </div>
                    <div class="col" align="left">
                      <label for="input1">select</label>
                      <div class="input-group mb-3">
                        <div class="input-group-prepend">
                          <span class="input-group-text" id="basic-addon1"
                            ><i class="fas fa-list-ul"></i
                          ></span>
                        </div>
                        <select v-model="borrow.status" required>
                          <option disabled value="">select</option>
                          <option value="pending review">pending review</option>
                          <option value="approved">approve</option>
                          <option value="disapproved">disapproved</option>
                          <option value="return">return</option>
                        </select>
                      </div>
                    </div>
                    <br />
                    <div class="row">
                      <div class="col">
                        <button
                          type="submit"
                          class="btn btn-success"
                          style="width: 100%"
                        >
                          <i class="far fa-save"></i> Save
                        </button>
                      </div>
                      <div class="col">
                        <button
                          class="btn btn-warning"
                          type="button"
                          style="width: 100%"
                          v-on:click="navigateTo('/borrow/status')"
                        >
                          <i class="fas fa-angle-double-left"></i> Back
                        </button>
                      </div>
                    </div>
                  </form>
                </div>
              </center>
            </div>
          </span>
        </div>
        <div class="footer"></div>
      </div>
    </div>
  </div>
</template>
<script>
import { mapState } from "vuex";
import BorrowsService from "@/services/BorrowsService";
import moment from "moment";

export default {
  data() {
    return {
      borrow: {
        nameLend: "",
        date: "",
        dateLend: "",
        dateReturn: "",
        equipment: "",
        number: "",
        status: "",
      },
    };
  },
  filters: {
    formatedDate(value) {
      return moment(String(value)).format("DD-MM-YYYY");
    },
  },
  computed: {
    ...mapState(["isUserLoggedIn", "user"]),
  },
  methods: {
    navigateTo(route) {
      this.$router.push(route);
    },
    async editBorrow() {
      try {
        await BorrowsService.put(this.borrow);
        this.$router.push({
          name: "borrow-status",
        });
      } catch (err) {
        console.log(err);
      }
    },
  },
  async created() {
    try {
      let borrowId = this.$route.params.borrowId;
      this.borrow = (await BorrowsService.show(borrowId)).data;
    } catch (error) {
      console.log(error);
    }
  },
};
</script>>

<style scoped>
.header {
  margin-left: auto;
  margin-right: auto;
  margin-top: 0px;
  padding: 10px;
  position: relative;
  background-color: #ececec;
  border-bottom: 1px solid rgba(255, 255, 255, 0.5);
  box-shadow: 0 -2px 1px rgba(0, 0, 0, 0.1) inset;
  text-shadow: 1px 1px 1px #fff;
}
.categories {
  margin-top: 10px;
  padding: 0;
  width: 100%;
}
.blog-wrapper {
  margin-top: 20px;
  padding: 40px;
  height: 100%;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}
.blog-tab {
  padding: 1px;
  background-color: #d3d3d3;
  text-align: left;
  text-indent: 0.5em;
}
.footer {
  margin-top: 50px;
}
</style>