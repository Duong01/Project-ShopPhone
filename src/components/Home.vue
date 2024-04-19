<template>
    <section class="banner container">
      <div class="slide container">
        <el-carousel :interval="2500" type="card" height="300px">
          <el-carousel-item>
            <img src="../assets/Files/slider/banner0.gif" alt="">
          </el-carousel-item>
          <el-carousel-item>
            <img src="../assets/Files/slider/banner1.png" alt="">
          </el-carousel-item>
          <el-carousel-item>
            <img src="../assets/Files/slider/banner2.png" alt="">
          </el-carousel-item>
          <el-carousel-item>
            <img src="../assets/Files/slider/banner3.png" alt="">
          </el-carousel-item>
          <el-carousel-item>
            <img src="../assets/Files/slider/banner4.png" alt="">  
          </el-carousel-item>
          <el-carousel-item>
            <img src="../assets/Files/slider/banner5.png" alt="">  
          </el-carousel-item>
          <el-carousel-item>
            <img src="../assets/Files/slider/banner6.png" alt="">
          </el-carousel-item>
          <el-carousel-item>
            <img src="../assets/Files/slider/banner7.png" alt="">
          </el-carousel-item>
          <el-carousel-item>
            <img src="../assets/Files/slider/banner8.png" alt="">
          </el-carousel-item>
          <el-carousel-item>
            <img src="../assets/Files/slider/banner9.png" alt="">
          </el-carousel-item>
        <!-- <h3 text="2xl" justify="center"></h3> -->
        </el-carousel>
      </div>
      <div class="qc">
        <img src="../assets/img/blackFriday.gif" alt="">
      </div>
      <div class="admin-right">
        <h3 class="tenKhung" style="background-image: linear-gradient(120deg, #ff9c00 0%, #ec1f1f 50%, #ff9c00 100%);">* {{$t('NỔI BẬT NHẤT')}} *</h3>
        <ListProduct/>
        <h3 class="tenKhung" style="margin: 30px 0 0 0;background-image: linear-gradient(120deg, #42bcf4 0%, #004c70 50%, #42bcf4 100%);">* {{$t('SẢN PHẨM MỚI')}} *</h3>
        <ProductNew />
        <h3 class="tenKhung" style="margin: 30px 0 0 0;background-image: linear-gradient(120deg, #5de272 0%, #007012 50%, #5de272 100%);">* {{$t('GIÁ SỐC ONLINE')}} *</h3>
        <PriceShock />
        <h3 class="tenKhung" style="margin: 30px 0 0 0;background-image: linear-gradient(120deg, #ff9c00 0%, #ec1f1f 50%, #ff9c00 100%);">* {{$t('GIẢM GIÁ LỚN')}} *</h3>
        <BigSaleOff />
        <el-backtop :right="100" :bottom="100" /> 
      </div>
    </section>
</template>

<script>
import ListProduct from '@/pages/ListProduct/ListProduct'
import ProductNew from '@/pages/ListProduct/ProductNew'
import PriceShock from '@/pages/ListProduct/PriceShock'
import BigSaleOff from '@/pages/ListProduct/BigSaleOff'
export default {
  name: 'HomePage',
  data () {
    return {
    }
  },
  components: {
    ListProduct,
    ProductNew,
    PriceShock,
    BigSaleOff
  }
}
</script>

<style scoped>
h3{
  display: block;
    line-height: 1.5em;
    font-size: 1.5em;
    text-align: center;
    margin: 0 auto;
    border-radius: 1em;
    color: white;
    transform: translateY(-0.75em);
    margin: 20px 0  0  0;
}
.qc{
  margin: 20px 0;
}
.banner {
  background-size: cover;
  margin-top: 30px;
  /* height: 500px; */
  overflow: hidden;
  margin: 0 auto;

}
.slide{
  position: relative;
  height: 300px;
  max-width: 1200px;
  margin: 0 auto;
  background: #fff;
}
img{
  border-radius: 10px;
  width:  100%;
  height: 100%;
}
.admin-right{
  width: 100%;
  min-height: 100px;
  float: left;
  margin: auto;
  
}

h4{
  text-align: center;
  margin: 10px 0;
  width: 100%;
  background-color: #01aa39;
  color: #fff;
  height: 40px;
  line-height: 40px;
}
</style>
