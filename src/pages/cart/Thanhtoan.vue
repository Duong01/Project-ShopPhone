<template>
  <main role="main">
    <!-- Block content - Đục lỗ trên giao diện bố cục chung, đặt tên là `content` -->
    <div class="container mt-4">
      <div class="text-center">
        <i class="fa fa-credit-card fa-4x" aria-hidden="true"></i>
        <h2>Thanh toán</h2>
        <p class="lead">
          Vui lòng kiểm tra thông tin Khách hàng, thông tin Giỏ hàng trước khi
          Đặt hàng.
        </p>
      </div>

      <div class="row">
        <div class="col-md-4 order-md-2 mb-4">
          <h4 class="d-flex justify-content-between align-items-center mb-3">
            <span class="text-muted">Giỏ hàng</span>
            <span
              style="
                color: #fff;
                width: 30px;
                height: 30px;
                border-radius: 30px;
                background-color: #cccccc;
              "
              class="badge badge-secondary badge-pill"
              >{{ count }}</span
            >
          </h4>
          <ul class="list-group mb-3">
            <li
              class="list-group-item d-flex justify-content-between lh-condensed"
              v-for="item in cart"
              :key="item.id"
            >
              <div class="text-left">
                <h6 class="my-0">{{ item.productName }}</h6>
                <small class="text-muted"
                  >{{ Number(item.productPrice).toLocaleString() }} x
                  {{ item.qty }}</small
                >
              </div>
              <div>
                <span class="text-right"
                  >{{
                    Number(item.productPrice * item.qty).toLocaleString()
                  }}đ</span
                >
              </div>
              <hr />
            </li>
            <li class="list-group-item d-flex justify-content-between">
              <span>{{ $t("Vận chuyển") }}</span>
              <strong>
                <el-tag size="small">Miễn phí vận chuyển</el-tag> <br />
                <p
                  style="
                    font-size: 12px;
                    border: 1px solid #ccc;
                    font-weight: normal;
                  "
                >
                  Tới: {{ user.address }}
                </p>
              </strong>
            </li>
            <li class="list-group-item d-flex justify-content-between">
              <span>{{ $t("Tổng tiền") }}</span>
              <strong>{{ cartTotal.toLocaleString() }}đ</strong>
            </li>
          </ul>
          <div class="input-group">
            <input
              type="text"
              class="form-control"
              placeholder="Mã khuyến mãi"
            />
            <div class="input-group-append">
              <button type="submit" class="btn btn-secondary">Xác nhận</button>
            </div>
          </div>
        </div>

        <div style="text-align: left" class="col-md-8 order-md-1">
          <h4 class="mb-3">Thông tin khách hàng</h4>

          <div class="row">
            <div class="col-md-12">
              <label for="kh_ten">Họ tên</label>
              <input
                type="text"
                class="form-control"
                name="kh_ten"
                id="kh_ten"
                :value="user.fullname"
                readonly=""
              />
            </div>
            <div class="col-md-12">
              <label for="kh_diachi">Địa chỉ</label>
              <input
                type="text"
                class="form-control"
                name="kh_diachi"
                id="kh_diachi"
                :value="user.address"
                readonly=""
              />
            </div>
            <div class="col-md-12">
              <label for="kh_dienthoai">Điện thoại</label>
              <input
                type="text"
                class="form-control"
                name="kh_dienthoai"
                id="kh_dienthoai"
                :value="user.phone"
                readonly=""
              />
            </div>
            <div class="col-md-12">
              <label for="kh_email">Email</label>
              <input
                type="text"
                class="form-control"
                name="kh_email"
                id="kh_email"
                :value="user.email"
                readonly=""
              />
            </div>
            <div class="col-md-12">
              <label for="kh_ngaysinh">Ngày sinh</label>
              <input
                type="text"
                class="form-control"
                name="kh_ngaysinh"
                id="kh_ngaysinh"
                :value="user.dob"
                readonly=""
              />
            </div>
          </div>

          <h4 class="mb-3">Hình thức thanh toán</h4>

          <div class="d-block my-3">
            <div class="custom-control custom-radio">
              <input
                id="httt-1"
                name="httt_ma"
                type="radio"
                v-model="radio"
                class="custom-control-input"
                required=""
                value="Thanh toán khi nhận hàng"
              />
              <label class="custom-control-label" for="httt-1"
                >Thanh toán khi nhận hàng</label
              >
            </div>
            <div class="custom-control custom-radio">
              <input
                id="httt-2"
                name="httt_ma"
                type="radio"
                v-model="radio"
                class="custom-control-input"
                required=""
                value="Chuyển khoản"
              />
              <label class="custom-control-label" for="httt-2"
                >Chuyển khoản ngân hàng</label
              >
            </div>
            <div class="custom-control custom-radio">
              <input
                id="httt-3"
                name="httt_ma"
                type="radio"
                v-model="radio"
                class="custom-control-input"
                required=""
                value="Ship COD"
              />
              <label class="custom-control-label" for="httt-3">Ship COD</label>
            </div>
          </div>
          <p style="color: #f59000">
            Lưu ý: Khách hàng mua hàng kiểm tra hàng trước khi thanh toán
          </p>
          <hr class="mb-4" />
          <button
            class="btn btn-primary btn-lg btn-block"
            type="button"
            @click="buy"
            name="btnDatHang"
          >
            Đặt hàng
          </button>
        </div>
      </div>
    </div>
    <!-- End block content -->
    <!-- Dialog -->
    <el-dialog
      v-model="openDialog"
      width="1000px"
      :fullscreen="true"
      :close-on-click-modal="false"
    >
      <div class="container">
        <div class="row">
          <!-- Phần thứ nhất -->
          <div class="col-md-8">
            <h4>SmartPhone Store</h4>
            <p style="color: orange">Cảm ơn bạn đã mua hàng!</p>
            <div class="row mt-4 border">
              <!-- Hướng dẫn chuyển khoản -->
              <div class="col-md-6">
                <p>Cách 1: Mở app ngân hàng và quét mã QR</p>
                <img
                  width="150"
                  height="150"
                  :src="`http://localhost:8085/Files/QRCode.jpg`"
                  alt="QR Code"
                  class="img-fluid mt-3"
                />
              </div>
              <!-- Thông tin đơn hàng -->
              <div class="col-md-6">
                <p>Cách 2: Chuyển khoản thủ công theo thông tin</p>
                <ul class="info-list text-left">
                  <li><strong>Tên ngân hàng:</strong> Ngân hàng Vietcombank</li>
                  <li><strong>Tên tài khoản:</strong> NGUYEN VAN DUONG</li>
                  <li>
                    <strong>Số tài khoản:</strong> 1039865635
                    <button
                      class="btn btn-sm text-primary copy-btn"
                      @click="copyToClipboard"
                    >
                      Sao chép
                    </button>
                  </li>
                  <li>
                    <strong>Số tiền:</strong> {{ cartTotal.toLocaleString() }}đ
                    <button
                      class="btn btn-sm text-primary copy-btn"
                      @click="copyToClipboard"
                    >
                      Sao chép
                    </button>
                  </li>
                  <li>
                    <strong>Nội dung:</strong> {{ randomContent }}
                    <button
                      class="btn btn-sm text-primary copy-btn"
                      @click="copyToClipboard"
                    >
                      Sao chép
                    </button>
                  </li>
                </ul>
              </div>
              <p style="color: #f59000">
                Lưu ý: Shop sẽ xác nhận chuyển khoản của bạn trong vòng 10 phút
              </p>
            </div>
            <div class="row mt-4 border">
              <h4>Thông tin đơn hàng</h4>
              <p>Thông tin giao hàng</p>
              <div class="col-md-12">
                <p>{{ user.fullname }}</p>
                <p>{{ user.address }}</p>
                <p>{{ user.phone }}</p>
                <p>{{ user.email }}</p>
              </div>
            </div>
          </div>
          <!-- Phần thứ hai -->
          <div class="col-md-4">
            <ul class="list-group mb-3">
              <li
                class="list-group-item d-flex justify-content-between lh-condensed"
                v-for="item in cart"
                :key="item.id"
              >
                <div class="text-left">
                  <img
                    class="img"
                    :src="`http://localhost:8085/Files/${item.productImage}`"
                    alt=""
                  />
                  <div class="product-info">
                    <h6 class="my-0">{{ item.productName }}</h6>
                    <small class="text-muted"
                      >{{ Number(item.productPrice).toLocaleString() }} x
                      {{ item.qty }}</small
                    >
                  </div>
                </div>
                <div style="margin-top: 70px">
                  <span class="text-center"
                    >{{
                      Number(item.productPrice * item.qty).toLocaleString()
                    }}đ</span
                  >
                </div>

                <hr />
              </li>
              <li class="list-group-item d-flex justify-content-between">
                <span>{{ $t("Vận chuyển") }}</span>
                <strong>
                  <el-tag size="small">Miễn phí vận chuyển</el-tag> <br />
                  <p
                    style="
                      font-size: 12px;
                      border: 1px solid #ccc;
                      font-weight: normal;
                    "
                  >
                    Tới: {{ user.address }}
                  </p>
                </strong>
              </li>
              <li class="list-group-item d-flex justify-content-between">
                <span>{{ $t("Tổng tiền") }}</span>
                <strong>{{ cartTotal.toLocaleString() }}đ</strong>
              </li>
            </ul>
          </div>
        </div>
      </div>
      <template #footer>
        <div class="dialog-footer centered-footer">
          <!-- <el-button @click="openDialog = false">Cancel</el-button> -->
          <el-button :plain="true" type="primary" @click="confirm">
            Xác nhận thanh toán
          </el-button>
        </div>
      </template>
    </el-dialog>
  </main>
</template>
<script>
import { ElMessage } from "element-plus";
import axios from "axios";
export default {
  name: "ThanhToan",
  data() {
    return {
      radio: "",
      user: [],
      cart: [],
      order: [],
      count: 0,
      idOrder: 0,
      name: "",
      address: "",
      phone: "",
      email: "",
      dob: "",
      openDialog: false,
      randomContent: "",
    };
  },
  created() {
    this.getAll(),
      this.getCart(),
      this.getNow(),
      (this.randomContent = this.generateRandomContent());
      
  },
  methods: {
    getAll() {
      axios
        .get(
          `http://localhost:8181/api/user/finduserbyid?id=${this.getEmpInfor}`
        )
        .then((res) => {
          if (res.status == 200 && res.data != null) {
            this.user = res.data;
          }
        });
    },
    getCart() {
      axios
        .get(
          `http://localhost:8181/api/cart/showcartbyuserid?userid=${this.getEmpInfor}`
        )
        .then((res) => {
          if (res.status == 200 && res.data != null) {
            this.cart = res.data;
            // this.soldCount;
            this.count = res.data.length;
          }
        });
    },
    getCurrentDate() {
      const date = new Date();
      const day = String(date.getDate()).padStart(2, "0");
      const month = String(date.getMonth() + 1).padStart(2, "0");
      const year = date.getFullYear();
      return `${day}/${month}/${year}`;
    },
    generateRandomContent() {
      const characters = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
      let result = "";
      const charactersLength = characters.length;
      for (let i = 0; i < 6; i++) {
        result += characters.charAt(
          Math.floor(Math.random() * charactersLength)
        );
      }
      return result;
    },
    copyToClipboard() {
      ElMessage({
        message: "Sao chép thành công",
        grouping: true,
        type: "success",
      });
    },
    getNow() {
      var today = new Date();
      var date =
        today.getFullYear() +
        "-" +
        (today.getMonth() + 1) +
        "-" +
        today.getDate();
      // var time =
      //   today.getHours() + ":" + today.getMinutes() + ":" + today.getSeconds();
      // var dateTime = date+'T'+ time;
      this.timenow = date;
    },
    confirm() {
      for (let i = 0; i < this.count; i++) {
        axios
          .post(`http://localhost:8181/api/order/inserorder`, {
            produuctName: this.cart[i].productName,
            userId: this.cart[i].userId,
            createDate: this.timenow,
            receivedDate: this.timenow,
            qty: this.cart[i].qty,
            price: this.cart[i].productPrice * this.cart[i].qty,
            status: "Processing",
          })
          .then((res) => {
            console.log(res);
            this.idOrder = res.data.id;
            // axios.put(`http://localhost:8181/api/products/updateqtyproduct`,{
            //   id: this.cart[i].productId,
            //   soldCount:  this.cart[i].qty
            // })
            axios
              .post(`http://localhost:8181/api/orderdetail/insertorderdetail`, {
                orderId: this.idOrder,
                productId: this.cart[i].productId,
                qty: this.cart[i].qty,
                productPrice: this.cart[i].productPrice,
                productName: this.cart[i].productName,
                productImage: this.cart[i].productImage,
                userName: this.user.fullname,
                userAddress: this.user.address,
                userPhone: this.user.phone,
                userEmail: this.user.email,
              })
              .then((res) => {
                if (res.data == "ok" && res.status == 200) {
                  ElMessage({
                    type: "success",
                    message: "Confirm successful payment",
                  });
                  this.$router.push("/order");
                }
              })
              .catch(() => {
                ElMessage({
                  type: "error",
                  message: "Order error",
                });
              });
          });
      }
      axios.delete(
        `http://localhost:8181/api/cart/deleteallcart?userId=${this.getEmpInfor}`
      );
    },
    buy() {
      if (this.radio == "") {
        ElMessage({
          message: "Check status salary.",
          grouping: true,
          type: "error",
        });
      } else {
        if (this.radio == "Chuyển khoản") {
          this.openDialog = true;
        } else {
          for (let i = 0; i < this.count; i++) {
            axios
              .post(`http://localhost:8181/api/order/inserorder`, {
                produuctName: this.cart[i].productName,
                userId: this.cart[i].userId,
                createDate: this.timenow,
                receivedDate: this.timenow,
                qty: this.cart[i].qty,
                price: this.cart[i].productPrice * this.cart[i].qty,
                status: "Processing",
              })
              .then((res) => {
                console.log(res);
                this.idOrder = res.data.id;
                // axios.put(`http://localhost:8181/api/products/updateqtyproduct`,{
                //   id: this.cart[i].productId,
                //   soldCount:  this.cart[i].qty
                // })
                axios
                  .post(
                    `http://localhost:8181/api/orderdetail/insertorderdetail`,
                    {
                      orderId: this.idOrder,
                      productId: this.cart[i].productId,
                      qty: this.cart[i].qty,
                      productPrice: this.cart[i].productPrice,
                      productName: this.cart[i].productName,
                      productImage: this.cart[i].productImage,
                      userName: this.user.fullname,
                      userAddress: this.user.address,
                      userPhone: this.user.phone,
                      userEmail: this.user.email,
                    }
                  )
                  .then((res) => {
                    if (res.data == "ok" && res.status == 200) {
                      ElMessage({
                        type: "success",
                        message: "Order success",
                      });
                      this.$router.push("/order");
                    }
                  })
                  .catch(() => {
                    ElMessage({
                      type: "error",
                      message: "Order error",
                    });
                  });
              });
          }
          axios.delete(
            `http://localhost:8181/api/cart/deleteallcart?userId=${this.getEmpInfor}`
          );
        }
      }
    },
  },
  computed: {
    getEmpInfor() {
      let emp = JSON.parse(localStorage["us"]);
      if (emp != undefined) return emp.id;
      return "";
    },
    cartTotal() {
      return this.cart
        .map((item) => item.productPrice * item.qty)
        .reduce((total, amount) => total + amount, 0);
    },
  },
};
</script>

<style>
.full-screen-dialog .el-dialog__wrapper {
  height: 100%;
  margin: 0;
  top: 0;
  left: 0;
}
.full-screen-dialog .el-dialog {
  height: 100%;
  width: 100%;
  max-width: none;
  margin: 0;
  padding: 0;
}
.centered-footer {
  display: flex;
  justify-content: center;
  align-items: center;
}
.info-list li {
  margin-bottom: 10px;
  list-style: none;
  text-align: left;
}
.info-list li strong {
  color: #343a40;
}
.btn {
  float: right;
}
.img {
  max-width: 70px;
  height: auto;
  margin-right: 20px;
}
</style>
