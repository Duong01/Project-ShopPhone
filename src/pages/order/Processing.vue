<template>
  <div class="ordering" v-loading="this.$store.state.isLoading">
    <table class="table table-bordered">
      <thead>
        <tr>
          <th scope="col">STT</th>
          <th scope="col">Mã đơn hàng</th>
          <th scope="col">Ngày đặt</th>
          <th scope="col">Ngày giao</th>
          <th scope="col">Tình trạng</th>
          <th scope="col">Thao tác</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in order" :key="index">
          <td>{{ index + 1 }}</td>
          <td>{{ item.id }}</td>
          <td>{{ formatDate(item.createDate) }}</td>
          <td>
            Dự kiến giao hàng {{ calculateDeliveryDate(item.createDate) }}
          </td>
          <td>{{ item.status }}</td>
          <td>
            <button
              class="btn btn-warning"
              :disabled="item.status != 'Processing'"
              plain
              @click="DeleteOrder(item)"
            >
              Hủy đơn hàng
            </button>
            <button class="btn btn-primary" @click="showDetail(item)">
              Detail
            </button>
          </td>
        </tr>
      </tbody>
    </table>

    <!-- Dialog -->
    <el-dialog
      v-model="dialogTableVisible"
      title="Chi tiết đơn hàng"
      border
      align="center"
      width="1200"
    >
      <el-steps
        style="max-width: 600px"
        :space="200"
        :active="active"
        align-center
        finish-status="success"
      >
        <el-step :active="1" title="Đang xác nhận" />
        <el-step title="Đã xử lý" />
        <el-step title="Đang vận chuyển" />
      </el-steps>
      <br />
      <el-table :data="order_details">
        <el-table-column prop="orderId" label="Mã đơn hàng" width="120" />
        <el-table-column prop="productId" label="Mã sản phẩm" width="120" />
        <el-table-column prop="productName" label="Tên sản phẩm" width="120" />
        <el-table-column prop="productPrice" label="Giá sản phẩm" width="120">
          <template v-slot="scope">
            <p>{{ Number(scope.row.productPrice).toLocaleString() }}đ</p>
          </template>
        </el-table-column>
        <el-table-column prop="qty" label="Số lượng" width="120" />
        <el-table-column prop="productImage" label="Hình ảnh" width="120">
          <template v-slot="scope">
            <img
              :src="`http://localhost:8085/Files/${scope.row.productImage}`"
              alt="Hình ảnh"
              style="max-width: 100px; max-height: 100px"
            />
          </template>
        </el-table-column>
        <el-table-column prop="userName" label="Họ tên" width="120" />
        <el-table-column prop="userAddress" label="Địa chỉ" width="150" />
        <el-table-column prop="userPhone" label="SDT" width="120" />
      </el-table>
    </el-dialog>
    <div style="text-align: center; font-weight: bold">{{ text }}</div>
  </div>
</template>

<script>
import { ElMessage, ElMessageBox } from "element-plus";
import axios from "axios";

export default {
  name: "ProcessingPage",
  data() {
    return {
      dialogTableVisible: false,
      order: [],
      order_details: [],
      text: "",
      active: 0,
    };
  },
  created() {
    this.getAll();
  },
  computed: {
    getEmpInfor() {
      let emp = JSON.parse(localStorage["us"]);
      if (emp != undefined) return emp.id;
      return "";
    },
  },
  methods: {
    formatDate(dateString) {
      const date = new Date(dateString);
      const day = String(date.getDate()).padStart(2, "0");
      const month = String(date.getMonth() + 1).padStart(2, "0");
      const year = date.getFullYear();
      return `${day}/${month}/${year}`;
    },
    // Date Time
    calculateDeliveryDate(dateString) {
      const date = new Date(dateString);
      date.setDate(date.getDate() + 3); // Cộng thêm 3 ngày
      const day = String(date.getDate()).padStart(2, "0");
      const month = String(date.getMonth() + 1).padStart(2, "0");
      const year = date.getFullYear();
      return `${day}/${month}/${year}`;
    },
    calculateDelivery(createDate) {
      const deliveryDate = new Date(createDate); // Tạo một đối tượng ngày từ ngày đặt hàng
      deliveryDate.setDate(deliveryDate.getDate()); // Thêm 3 ngày vào ngày đặt hàng
      return deliveryDate.toISOString().slice(0, 10); // Trả về ngày giao hàng dự kiến dưới dạng YYYY-MM-DD
    },
    getAll() {
      axios
        .get(
          `http://localhost:8181/api/order/listorderbystatus?userId=${this.getEmpInfor}`
        )
        .then((res) => {
          if (res.status == 200) {
            this.order = res.data.filter((order) => order.status !== "Success");

            this.$store.state.isLoading = false;
          }
          if (this.order == "") {
            this.text = "Không có đơn hàng nào cần sử lý";
          }
        });
      this.$store.state.isLoading = true;
    },

    DeleteOrder(item) {
      console.log(item);
      ElMessageBox.confirm(
        "Are you sure you want to cancel your order?",
        "Warning",
        {
          confirmButtonText: "OK",
          cancelButtonText: "Cancel",
          type: "warning",
        }
      )

        .then(() => {
          axios
            .delete(
              `http://localhost:8181/api/orderdetail/deleteorderdetail?orderId=${item.id}`
            )
            .then(() => {})
            .catch(() => {});
          axios
            .delete(`http://localhost:8181/api/order/deleteorder?id=${item.id}`)
            .then((res) => {
              if (res.data == true && res.status == 200) {
                ElMessage({
                  type: "success",
                  message: "Delete completed",
                });
                this.getAll();
              }
            })
            .catch(() => {
              ElMessage({
                type: "error",
                message: "Delete error",
              });
            });
        })
        .catch(() => {
          ElMessage({
            type: "info",
            message: "Delete canceled",
          });
        });
    },

    showDetail(item) {
      this.dialogTableVisible = true;
      axios
        .get(
          `http://localhost:8181/api/orderdetail/listorderdetail?orderId=${item.id}`
        )
        .then((res) => {
          if (res.status == 200) {
            this.order_details = res.data;
          }
        })
        .catch((err) => {
          alert("Co loi", err);
        });
      if (item.status == "Processing") {
        this.active = 1;
      }
      if (item.status == "Processed") {
        this.active = 2;
      }
      if (item.status == "Ordering") {
        this.active = 3;
      }
    },
  },
};
</script>

<style></style>
