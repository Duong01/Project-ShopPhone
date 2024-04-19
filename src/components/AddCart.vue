<template>
  <el-dialog
    v-model="dialogVisible"
    title="Add to cart"
    width="800px"
    @close="closeDialogAndNavigateBack"
    :close-on-click-modal="false"
  >
    <div>
      <el-image
        style="
          width: 300px;
          height: 300px;
          border: 1px solid #ccc;
          border-radius: 10px;
        "
        :src="`http://localhost:8085/Files/${productForm.image}`"
        :zoom-rate="1.2"
        :max-scale="7"
        :min-scale="4"
        :initial-index="4"
        fit="cover"
      />
      <p style="color: red">
        Giá:
        {{ Number(productForm.promotionPrice).toLocaleString() }}đ
      </p>
    </div>
    <el-form>
      <el-radio-group v-model="radio" size="large">
        <el-radio-button label="Đen" value="Đen" />
        <el-radio-button label="Trắng" value="Trắng" />
        <el-radio-button label="Đỏ" value="Đỏ" />
      </el-radio-group>
      <br />
      <br />
      <el-input-number v-model="num" :min="1" />
    </el-form>
    <template #footer>
      <div class="dialog-footer">
        <el-button @click="dialogVisible = false">Cancel</el-button>
        <el-button :plain="true" type="primary" @click="addcart(productForm)">
          Add cart
        </el-button>
      </div>
    </template>
  </el-dialog>
</template>


<script>
import { ElMessage } from "element-plus";
import axios from "axios";
import { ref } from "vue";
export default {
  name: "MailPage",
  data() {
    return {
      dialogVisible: true,
      productForm: [],
      radio: ref("Đen"),
      num: 1,
    };
  },
  created() {
    this.showProduct();
  },
  methods: {
    showProduct() {
      axios
        .get(
          `http://localhost:8181/api/products/findproductbyid?id=${this.$route.params.id}`
        )
        .then((res) => {
          if (res.status == 200) {
            this.productForm = res.data;
          }
        });
    },
    addcart(item) {
      if (this.show) {
        axios
          .post(`http://localhost:8181/api/cart/insertcart`, {
            userId: this.getEmpInfor,
            productId: item.id,
            qty: this.num,
            productName: item.name,
            productPrice: item.promotionPrice,
            productImage: item.image,
            color: this.radio
          })
          .then((res) => {
            if (res.data == "ok" && res.status == 200) {
              ElMessage({
                message: "Add cart success.",
                grouping: true,
                type: "success",
              });
            }
          });
        this.dialogVisible = false;
        this.wrapperVisible = false;
      } else {
        ElMessage.error("You are not loggin");
      }
    },
    closeDialogAndNavigateBack() {
      this.dialogVisible = false;
      this.$router.go(-1); // Chuyển hướng người dùng về trang trước đó
    },
  },
  computed: {
    getEmpInfor() {
      let emp = JSON.parse(localStorage["us"]);
      if (emp != undefined) return emp.id;
      return "";
    },
    show() {
      return localStorage.getItem("us") != undefined;
    },
  },
};
</script>
