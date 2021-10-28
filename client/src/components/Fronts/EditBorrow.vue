<template>
  <div>
    <main-header navsel="front"></main-header>
    <div class="header">
      <h3><i class="fas fa-edit"></i> แก้ไข</h3>
    </div>
    <div class="container-fluid">
      <div class="container">
        <div class="blog-wrapper">
          <h4><i class="far fa-clipboard"></i> ทำการแก้ไข</h4>
          <hr />
          <br />
          <span class="font2">
            <form v-on:submit.prevent="editBorrow">
              <div class="row">
                <div class="col">
                  <label for="input1">ตรวจสอบชื่อ</label>
                  <div class="input-group mb-3">
                    <div class="input-group-prepend">
                      <span class="input-group-text" id="basic-addon1"
                        ><i class="far fa-user"></i
                      ></span>
                    </div>
                    <input
                      required
                      type="text"
                      class="form-control"
                      placeholder="Name"
                      v-model="borrow.nameLend"
                      disabled
                    />
                  </div>
                </div>
               
              </div>
              <div class="row">
                <div class="col">
                  <label for="input1">วันที่ต้องการเปลี่ยน</label>
                  <div class="input-group mb-3">
                    <div class="input-group-prepend">
                      <span class="input-group-text" id="basic-addon1"
                        ><i class="far fa-calendar-alt"></i
                      ></span>
                    </div>
                    
                    <input
                      required
                      type="date"
                      class="form-control"
                      placeholder="Date"
                      v-model="borrow.dateLend"
                    />
                  </div>
                </div>
                
              </div>
              <div class="row">
                <div class="col">
                  <label for="input1">เวลาที่ต้องการเปลี่ยน</label>
                  <div class="input-group mb-3">
                    <div class="input-group-prepend">
                      <span class="input-group-text" id="basic-addon1"
                        ><i class="fas fa-boxes"></i
                      ></span>
                    </div>
                    <p style="white-space: pre-line;">{{ message }}</p>
<br>
<textarea v-model="message" placeholder="ใส่เวลาที่ต้องการจอง"></textarea>
                  </div>
                </div>
               
              </div>
            
              <div class="row">
                <div class="col">
                  <button
                    type="submit"
                    class="btn btn-success"
                    style="width: 100%"
                  >
                    <i class="fas fa-clipboard-check"></i> ยืนยัน
                  </button>
                </div>
                <div class="col">
                  <button
                    class="btn btn-danger"
                    type="button"
                    style="width: 100%"
                    v-on:click="navigateTo('/borrow/status')"
                  >
                    <i class="fas fa-times-circle"></i> ยกเลิก
                  </button>
                </div>
              </div>
            </form>
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
import BlogsService from "@/services/BlogsService";

export default {
  data() {
    return {
      blogs: [],
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
      this.blogs = (await BlogsService.index()).data;
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