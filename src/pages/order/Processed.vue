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
        </tr>
      </thead>
      <tbody>
        <tr v-for="item,index in order" :key="index">
          <td>{{ index +1 }}</td>
          <td>{{ item.id }}</td>
          <td>{{ formatDate(item.createDate) }}</td>
          <td>{{ calculateDeliveryDate(item.createDate) }}</td>
          <td>{{ item.status }}</td>
        </tr>
      </tbody>
    </table>
    <div style="text-align: center; font-weight: bold;">{{text}}</div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'ProcessedPage',
  data(){
    return{
      order: [],
      text: '',
      name: 'Processed'
    }
  },
  created(){
    this.getAll()
  },
  computed: {
    getEmpInfor()
    {
       let emp = JSON.parse( localStorage["us"])
       if(emp!=undefined)
       return emp.id;
      return ""
    },
  },
  methods: {
    // Date Time
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
            this.order = res.data.filter((order) => order.status == "Processed");

            this.$store.state.isLoading = false;
          }
          if (this.order == '') {
            this.text = "Không có đơn hàng nào cần sử lý";
          }
        });
      this.$store.state.isLoading = true;
    },
    showDetail(item){
      this.$confirm(`  
      <table class="table table-bordered" style="width:100%">
      <thead>
        <tr>
          <th scope="col">Mã đơn hàng</th>
          <th scope="col">Ngày đặt</th>
          <th scope="col">Ngày giao</th>
          <th scope="col">Tình trạng</th>
          <th scope="col">Thao tác</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>${ item.id }</td>
          <td>${ item.createDate }</td>
          <td>${ item.receivedDate }</td>
          <td>${ item.status }</td>
        </tr>
      </tbody>
    </table>`, {
        dangerouslyUseHTMLString: true,
        confirmButtonText: 'OK',
        cancelButtonText: 'cancel'
      })
      //   .then(() => {
      //     this.$router.push( `/orderDetail`)
      //   })
       
      // this.$router.push({ name: item.path })
      
    }
  }
}
</script>

<style>

</style>