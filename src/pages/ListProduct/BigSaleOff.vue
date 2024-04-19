<template>
  <div
    class="container"
    style="grid-template-columns: auto auto auto auto auto !important"
    v-loading="this.$store.state.isLoading"
  >
    <li class="sanpham" v-for="item in products" :key="item.id">
      <div class="card">
        <router-link :to="{ name: 'ProductDetail', params: { id: item.id } }">
          <el-badge value="Mới ra mắt" class="item" style="font-size: 20px">
            <div class="imgBx">
              <img
                style="width: 200px; height: 200px"
                :src="`http://localhost:8085/Files/${item.image}`"
              />
            </div>
          </el-badge>
          <div class="content">
            <div class="productName">
              <a style="font-size: 14px" class="card-title">
                <b>{{ item.name }}</b></a
              >
            </div>
            <div class="price">
              <strong
                >{{ Number(item.promotionPrice).toLocaleString() }}đ</strong
              >
              <span>{{ Number(item.originalPrice).toLocaleString() }}đ</span>
            </div>

            <div class="rate">
              <el-rate
                v-model="value"
                disabled
                size="small"
                show-score
                text-color="#ff9900"
                score-template="{value} points"
              />
            </div>
          </div>
        </router-link>
        <div class="action">
          <router-link :to="{name: 'AddCart',params: {id: item.id}}">
          <div class="action">
              <button class="add-cart">+</button>
            
            <span class="tooltiptext"> Add cart </span>
          </div>
        </router-link>
        </div>
      </div>
    </li>


  </div>
</template>

<script>
import axios from "axios";
import { ElMessage } from "element-plus";
export default {
  name: "BigSaleOff",
  data() {
    return {
      products: [],
      productForm: [],
      value: 4.5,
      display: false,
      num: 1,
      radio: "",
      dialogVisible: false,
      state: "",
    };
  },
  created() {
    this.getAll();
  },
  filters: {
    toUSD(value) {
      return `$${value.toLocaleString()}`;
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
  methods: {
    getAll() {
      axios
        .get("http://localhost:8181/api/products/listproducthot?status=4")
        .then((res) => {
          if (res.data != null) {
            this.products = res.data.slice(0, 10);
            this.$store.state.isLoading = false;
          } else {
            res.data = "Không có sản phẩm nào";
          }
        });
      this.$store.state.isLoading = true;
    },
    handSelect() {},
    querySearchAsync() {
      this.getAll();
    },
    Show(item) {
      this.productForm = [item.data];
      this.display = true;
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
    showfrom() {
      document.getElementById("form").style.display = "block";
    },
    close() {
      document.getElementById("form").style.display = "none";
    },
    openDialog(item) {
      this.productForm = item;
      this.dialogVisible = true;
    },
    closeDialog() {
      this.dialogVisible = false;
    },
  },
};
</script>

<style scoped>
li {
  width: 223px;
  /* height: 310px; */
  list-style: none;
  display: inline-block;
}
li a {
  text-decoration: none;
}

.container {
  position: relative;
  width: 100%;
  max-width: 1200px;
  display: grid;
  grid-template-columns: auto auto auto auto auto !important ;
  grid-gap: 20px;
  padding: 30px;
  z-index: 0;
  margin: auto;
  justify-content: center;
  justify-items: start;
  border: 2px solid #ff4d00;
  border-radius: 10px;
}

.container .card {
  width: 100%;
  background: #fff;
  border-radius: 10px;
  box-shadow: 2px 2px 2px gray;
  height: auto;
  grid-gap: 20px;
  margin: 20px 0;
}

.imgBx {
  position: relative;
  width: 200px;
  height: 200px;
  margin-top: 10px;
  overflow: hidden;
}

.imgBx img {
  text-align: center;
  object-fit: cover;
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  transition: 0.5s;
}

.container .card .imgBx:hover {
  transition: 0.5s;
  transform: scale(1.1);
}

.container .card .content {
  margin: 0;
}

.productName a {
  color: #6b00a8;
  display: -webkit-box;
  -webkit-line-clamp: 1; /* số dòng hiển thị */
  -webkit-box-orient: vertical;
  overflow: hidden;
  text-overflow: ellipsis;
  color: #000;
  /* margin-top:10px; */
}

.container .card .content .price {
  /* text-align: center; */
  margin: 10px;
  font-size: 14px;
  float: left;
}
.container .card .content .price span {
  margin: 10px;
  color: #000;
  text-decoration: line-through;
}
.container .card .content .price strong {
  color: red;
}
.container .card .content .original-price {
  color: darkred;
  text-align: center;
  margin-right: 10px;
  text-decoration: line-through;
  font-size: 15px;
}

.action {
  position: absolute;
  display: inline-block;
  bottom: 5px;
  right: 5px;
}
.action:hover {
  transition: 0.2s;
  transform: scale(1.1);
}
.action .tooltiptext {
  visibility: hidden;
  width: 100px;
  height: 30px;
  line-height: 30px;
  background-color: #555;
  color: #fff;
  text-align: center;
  font-size: 13px;
  border-radius: 6px;
  /* padding: 5px; */
  position: absolute;
  z-index: 21;
  bottom: 150%;
  left: 50%;
  margin-left: -75px;
  opacity: 0;
  transition: opacity 0.3s;
}
.action .tooltiptext::after {
  content: "";
  position: absolute;
  top: 100%;
  left: 50%;
  margin-left: -5px;
  border-width: 5px;
  border-style: solid;
  border-color: #555 transparent transparent transparent;
}
.action .tooltiptext:hover {
  display: block;
  content: "";
  position: absolute;
  top: 100%;
  left: 50%;
  margin-left: -5px;
  border-width: 5px;
  border-style: solid;
  border-color: #555 transparent transparent transparent;
}
.action:hover .tooltiptext {
  visibility: visible;
  opacity: 1;
}
.rate {
  float: left;
}
button.add-cart {
  width: 40px;
  height: 40px;
  border-radius: 40px;
  float: right;
}

</style>
