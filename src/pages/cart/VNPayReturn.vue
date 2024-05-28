<template>
  <div class="payment-result">
    <div class="result-card" :class="{'success': isSuccess, 'failure': !isSuccess}">
      <h1>Kết quả thanh toán</h1>
      <img :src="imageUrl" alt="Result Image" />
      <p>{{ message }}</p>
      <router-link to="/" class="home-link">Quay về trang chủ</router-link>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      message: '',
      isSuccess: false,
      imageUrl: ''
    };
  },
  created() {
    const params = new URLSearchParams(window.location.search);
    const responseCode = params.get('vnp_ResponseCode');
    if (responseCode === '00') {
      this.message = 'Thanh toán thành công!';
      this.isSuccess = true;
      this.imageUrl = require('@/assets/success.png'); // Adjust the path as needed
    } else {
      this.message =  'Thanh toán thất bại!';
      this.isSuccess = false;
      this.imageUrl = require('@/assets/failure.png'); // Adjust the path as needed
    }
  }
};
</script>

<style scoped>
.payment-result {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 70vh;
  background-color: #f4f4f4;
}

.result-card {
  background: #fff;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  padding: 20px;
  text-align: center;
  max-width: 400px;
  width: 100%;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.result-card.success {
  border-bottom: 10px solid #4caf50;
}

.result-card.failure {
  border-bottom: 10px solid #f44336;
}

.result-card h1 {
  font-size: 24px;
  margin-bottom: 10px;
}

.result-card p {
  font-size: 18px;
  margin-bottom: 20px;
}

.result-card img {
  width: 100px;
  height: 100px;
  margin-bottom: 20px;
}

.result-card .home-link {
  display: inline-block;
  padding: 10px 20px;
  border: none;
  background-color: #3f51b5;
  color: #fff;
  text-decoration: none;
  border-radius: 5px;
  font-size: 16px;
  transition: background-color 0.3s ease;
}

.result-card .home-link:hover {
  background-color: #303f9f;
}
</style>
