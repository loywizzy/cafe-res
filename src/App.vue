<template>
  <div>
    <header>
    <nav class="navbar navbar-expand">
      <a class="navbar-brand" href="#">Cafe Reservation <i class="fa-solid fa-mug-hot"></i></a>
      <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav"
        aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav ml-auto">
          <li class="nav-item">
            <a class="nav-link" href="#dddd">ข้อมูลการจองร้าน <i class="fa-solid fa-chevron-down"></i></a>
          </li>
        </ul>
      </div>
    </nav>
    </header>

    <div class="container">
      <div class="row">
        <div class="col-md-3" v-for="(i, index) in travellist" :key="index">
          <div class="card , " style="width: 17rem;">
            <img :src="i.img" class="card-img-top" alt="...">
            <div class="card-body">
              <h5 class="card-title">{{ i.name }}</h5>
              <p class="card-text">ราคา: {{ i.price }} บาท</p>
              <form @submit.prevent="booking(i.name, bookingForm)">
                <div class="form-group">
                  <label for="name">ชื่อผู้จอง</label>
                  <input type="text" class="form-control" placeholder="กรอกชื่อ" v-model="bookingForm.name" required>
                </div>
                <div class="form-group">
                  <label for="phone">เบอร์โทร</label>
                  <input type="text" class="form-control" pattern="[0-9]{10}" placeholder="0XXXXXXXXX"
                    v-model="bookingForm.phone" required>
                </div>
                <div class="form-group">
                  <label for="datetime">วันที่ เวลา</label>
                  <input type="datetime-local" class="form-control" v-model="bookingForm.datetime" required>
                </div>
                <div class="form-group">
                  <label for="table">จำนวนโต๊ะ</label>
                  <input type="number" class="form-control" v-model="bookingForm.tables" pattern="[0-1]{2}" required>
                </div><br>
                <button type="submit" class="btn btn-outline-success">จอง</button>
              </form>
            </div>
          </div>
        </div>
      </div>
    </div>

    <br>
    <div>
      <h3 id="dddd">ข้อมูลการจองร้าน</h3>
      <table class="table table-striped" v-if="bookinglist.length">
        <thead class="table-light">
          <tr>
            <th scope="col">#</th>
            <th scope="col">ชื่อร้าน</th>
            <th scope="col">ชื่อผู้จอง</th>
            <th scope="col">เบอร์โทร</th>
            <th scope="col">วันที่ และ เวลา</th>
            <th scope="col">จำนวนโต๊ะ</th>
          </tr>
        </thead>
        <tbody class="table-group-divider">
          <tr v-for="(item, index) in bookinglist" :key="index">
            <th scope="row">{{ index + 1 }}</th>
            <td>{{ item.restaurant }}</td>
            <td>{{ item.name }}</td>
            <td>{{ item.phone }}</td>
            <td>{{ item.datetime }}</td>
            <td>{{ item.tables }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const bookinglist = ref([]);

const bookingForm = ref({
  name: '',
  phone: '',
  datetime: '',
  tables: 0,
});

function booking(restaurantName, formData) {
  // ตรวจสอบว่าข้อมูลถูกกรอกครบถ้วน
  if (
    formData.name.trim() !== '' &&
    formData.phone.trim() !== '' &&
    formData.datetime.trim() !== '' &&
    formData.tables > 0
  ) {
    // เพิ่มข้อมูลการจองลงใน bookinglist
    bookinglist.value.push({
      restaurant: restaurantName,
      name: formData.name,
      phone: formData.phone,
      datetime: formData.datetime,
      tables: formData.tables,
    });

    // รีเซ็ตฟอร์ม
    formData.name = '';
    formData.phone = '';
    formData.datetime = '';
    formData.tables = 0;
  } else {
    alert('กรุณากรอกข้อมูลให้ครบถ้วน');
  }
}


const travellist = ref([
  { name: 'Campus Coffee Roaster', price: 100, size: { s: 99, m: 129, l: 169 }, img: "https://cdn.phuket.net/bucket/mainsite/size/700/393/2021/09/campus-03.jpg" },
  { name: 'Sync Coffee Phuket', price: 150, size: { s: 99, m: 129, l: 169 }, img: "https://cdn.phuket.net/bucket/mainsite/size/700/393/2021/09/cafe-83000.jpg" },
  { name: 'Element Café By Lafayette 7', price: 300, size: { s: 99, m: 129, l: 169 }, img: "https://cdn.phuket.net/bucket/mainsite/size/700/393/2021/09/Element-Cafe-By-Lafayette-7-01.jpg" },
  { name: 'Angpao Cafe & Restaurant', price: 800, size: { s: 99, m: 129, l: 169 }, img: "https://cdn.phuket.net/bucket/mainsite/size/700/393/2021/09/angpao-03.jpg" },
]);
</script>

<style scoped>
card {
  color: rgb(28, 104, 171);
}

</style>

