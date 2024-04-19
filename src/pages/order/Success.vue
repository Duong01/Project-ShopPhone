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
          <td>{{ item.createDate }}</td>
          <td>{{ item.receivedDate }}</td>
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
  name: 'SuccessPage',
  data(){
    return{
      order: [],
      text: '',
      name: 'Success'
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
    getAll() {
        axios.get(`http://localhost:8181/api/order/listorderbystatus?status=${this.name}&userId=${this.getEmpInfor}`)
        .then((res) => {
          if(res.status == 200){
            this.order = res.data
            this.$store.state.isLoading = false
          }
          if(res.data.length == 0){
            this.text = 'Không có đơn hàng nào cần sử lý'
            }
        })
        this.$store.state.isLoading = true
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