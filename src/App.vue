<template>
  <br><br>
  <div class="container">
    <span class="me-2">+ Tambah Karyawan</span>
    <button class="btn btn-primary"  data-bs-toggle="modal" data-bs-target="#modalBox">Rekrut</button>
    <br>
    <h1>Data Karyawan.</h1>
    <br>
    <table class="table">
      <thead>
        <tr>
          <th scope="col">No.</th>
          <th scope="col">Foto</th>
          <th scope="col">Depan</th>
          <th scope="col">Belakang</th>
          <th scope="col">Email</th>
          <th scope="col">Interaksi</th>
        </tr>
      </thead>
      <tbody v-for="(orang, i) in karyawan" :key="i">
        <tr>
          <th scope="row">{{ i + 1 }}</th>
          <td><img :src="orang.avatar" alt="orang" style="width: 50px;" data-bs-toggle="modal" data-bs-target="#detail" @click="e => viewKaryawan(orang.id)"></td>  
          <td>{{ orang.first_name }}</td>
          <td>{{ orang.last_name }}</td>
          <td>{{ orang.email }}</td>
          <td>
            <button class="btn btn-warning me-2">Ubah</button>
            <button class="btn btn-outline-danger" @click="() => deleteKaryawan(orang.id)">Pecat</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>

  <!-- Modal -->
  <div class="modal fade" id="modalBox" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h1 class="modal-title fs-5" id="exampleModalLabel">Rekrut Karyawan</h1>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          <form @submit.prevent="submitForm">
            <div class="mb-3">
              <label for="foto" class="form-label">Foto</label>
              <input type="file" class="form-control" @change="(e) => this.target = e.target.files[0]" id="foto" required>
            </div>
            <div class="mb-3">
              <label for="nama" class="form-label">Nama</label>
              <input type="text" class="form-control" v-model="name" id="nama" required>
            </div>
            <div class="mb-3">
              <label for="exampleInputEmail1" class="form-label">Email address</label>
              <input type="email" class="form-control" v-model="email" id="exampleInputEmail1" aria-describedby="emailHelp" required>
              <div id="emailHelp" class="form-text">We'll never share your email with anyone else.</div>
            </div>
            <button type="submit" class="btn btn-primary">Submit</button>
          </form>
        </div>
      </div>
    </div>
  </div>

  <div class="modal fade" id="detail" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h1 class="modal-title fs-5" id="exampleModalLabel">Detail Data</h1>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          <ul class="list-group">
            <li class="list-group-item"><img :src="davatar" alt="orang"></li>
            <li class="list-group-item">{{  dname  }}</li>
            <li class="list-group-item">{{ demail }}</li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// fetch("https://reqres.in/api/users?page=2")
//   .then(res => res.json()) // Chaining
//   .then(res => console.log(res))

import axios from 'axios'
// let axios = require('axios')

export default {
  name: 'App',
  data(){
    return {
      karyawan : [],
      name: '',
      email: '',
      avatar: null,
      dname: '',
      demail: '',
      davatar: ''
    }
  },
  mounted(){
    // Get Metode untuk ambil data
    axios.get("https://reqres.in/api/users?page=2")
      .then(res => (this.karyawan = res.data.data, console.log(res.data.data[2]))) 
  },
  methods: {
    submitForm(){
      let formData = new FormData();

      formData.append('name', this.name)
      formData.append('email', this.email)
      formData.append('avatar', this.avatar)

      axios.post("https://reqres.in/api/users", formData, {
        headers: {
          'Content-Type': 'multipart/form-data'
        }
      })
        .then(res => console.log("Berhasil Ditambahkan : ", res))
        .catch(err => console.error("Semua salah bang hanang : ", err))
    },
    deleteKaryawan(params){
      axios.delete('https://reqres.in/api/users/'+params)
        .then(res => (console.log("Karena Bang hanang, karyawan dipecat"),console.log(res)))
    },
    viewKaryawan(params){
      axios.get('https://reqres.in/api/users/'+params)
        .then(res => {
          this.dname = res.data.data.first_name + ' ' + res.data.data.last_name
          this.davatar = res.data.data.avatar
          this.demail = res.data.data.email
        })
    }
  }
}
</script>
