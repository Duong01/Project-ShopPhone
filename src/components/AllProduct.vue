<template>
  <div class="container" v-loading="this.$store.state.isLoading">
  <div class="row">
    <div
      class="col-md-3 mb-4"
      v-for="item in products"
      :key="item.id"
    >
      <div class="card">
        <router-link :to="{ name: 'ProductDetail', params: { id: item.id } }">
          <el-badge value="hot" class="item" style="font-size:20px">
            <div class="imgBx">
              <img
                class="card-img-top"
                :src="`http://localhost:8085/Files/${item.image}`"
                alt="Product Image"
                
              />
            </div>
          </el-badge>
          <div class="card-body">
            <div class="productName">
              <a style="font-size: 14px;" class="card-title">
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
                v-model="item.status"
                disabled
                size="small"
                show-score
                text-color="#ff9900"
                score-template="{value} points"
              />
            </div>
          </div>
        </router-link>
        <router-link :to="{name: 'AddCart',params: {id: item.id}}">
          <div class="action">
              <button class="add-cart">+</button>
            
            <span class="tooltiptext"> Add cart </span>
          </div>
        </router-link>
      </div>
    </div>
  </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: 'TestPage',
  data() {
    return {
      userInput: "",
      messages: [],
    };
  },
  created() {
    this.getAll();
  },
  methods: {
    getAll() {
      axios
        .get("http://localhost:8181/api/products/listproduct")
        .then((res) => {
          if (res.data != null) {
            const filteredProducts = res.data.filter(product => {
              const productId = parseInt(product.id);
              return productId < 91 || productId > 139;
            });
            const randomProducts = this.shuffleArray(filteredProducts).slice(0, 20);
            this.products = randomProducts;
            this.$store.state.isLoading = false;
          } else {
            res.data = "Không có sản phẩm nào";
          }
        });
      this.$store.state.isLoading = true;
    },
    shuffleArray(array) {
      // Phương thức để xáo trộn mảng
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
      return array;
    }
  },
};
</script>

<style scoped>

.xemTatCa {
    display: flex;
    font-weight: bold;
    text-align: center;
    margin: 0 auto;
    padding: .5em 1em;
    border-radius: 1em;
    color: #888;
    background-color: #eee;
    transition-duration: .2s;
    transform: translateY(1em);
    border-left: 2px solid #42bcf4;
    border-right: 2px solid #42bcf4;
}
.xemTatCa:hover {
    background-color: #ccc;
    color: #000;
}
a{
    text-decoration: none;
    color: #222;
}
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
  /* display: grid;
  grid-template-columns: auto auto auto auto auto !important ;
  grid-gap: 20px; */
  padding: 30px;
  z-index: 0;
  margin: auto;
  justify-content: center;
  justify-items: start;
  border: 2px solid #ff4d00;
  border-radius: 10px;
}

.container .card {
  width: 95%;
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
  transition: 0.7s;
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

.container .card .price {
  /* text-align: center; */
  margin: 10px;
  font-size: 14px;
  float: left;
}
.container .card .price span {
  margin: 10px;
  color: #000;
  text-decoration: line-through;
}
.container .card .price strong {
  color: red;
}
.container .card .original-price {
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