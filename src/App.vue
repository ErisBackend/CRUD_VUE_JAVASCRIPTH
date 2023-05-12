<template>
  <br><br>
  <div class="container">
    <h1>Data Kariawan</h1>
    <span class="me-2">+ Tambah Karyawan</span> 
    <button class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#exampleModal" @click="() => this.Judul = 'Tambah Karyawan'">Add</button>
    <!-- data-bs-toggle="modal" data-bs-target="#exampleModal" dari bootstrap perintah untuk membuta pop up modal -->
    <br>
    <table class="table">
  <thead>
    <tr>
      <th scope="col">NO</th>
      <th scope="col">Poto</th>
      <th scope="col">Depan</th>
      <th scope="col">Belakang</th>
      <th scope="col">Email</th>
      <th scope="col">Interaki</th>

    </tr>
  </thead>
  <tbody v-for="(orang,i) in karyawan" :key="i"> 
    <!-- (orang,i) adalah index dari data -->
    <!-- :key ='i' adalah kunci loopingan  -->
    <tr>
      <th scope="row">{{ i + 1 }}</th>
      <td><img :src="orang.avatar" alt="orang" data-bs-toggle="modal" data-bs-target="#detail" @click="e => ViewKaryawan(orang.id)"  style="width: 50px;"></td>
      <td>{{ orang.first_name }}</td>
      <td>{{ orang.last_name }}</td>
      <td>{{ orang.email }}</td>
      <td>
        <button  class="btn btn-primary me-2" data-bs-toggle="modal" data-bs-target="#exampleModal" @click="() => ubahKaryawan(orang.id, orang.first_name , orang.last_name, orang.email)">ubah</button>
        <button class="btn btn-danger" @click="() => hapus(orang.id)">pecat</button>
      </td>
    </tr>
    
  </tbody>
</table>
  </div>

  <!-- Modal yang di ambil dari ootstrap-->
<div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">{{ Judul }}</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
        <form @submit.prevent="submitForm">
          <div class="mb-3">
            <label for="avatar" class="form-label">Poto</label>
            <input type="file" class="form-control" @change="(e) => this.target = e.target.files[0]" id="foto">
            <!-- <div id="emailHelp" class="form-text">We'll never share your email with anyone else.</div> -->
          </div>
          <div class="mb-3">
            <label for="nama_depan" class="form-label">Nama</label>
            <input type="text" class="form-control" id="nama_depan" :value="Fname" required>
            <!-- <div id="emailHelp" class="form-text">We'll never share your email with anyone else.</div> -->
          </div>
         
          <div class="mb-3">
            <label for="email" class="form-label">Email</label>
            <input type="email" class="form-control" id="email" :value="Femail" required>
            <!-- <div id="emailHelp" class="form-text">We'll never share your email with anyone else.</div> -->
          </div>
          <button type="submit" class="btn btn-primary">Submit</button>
        </form>
      </div>
    </div>
  </div>
</div>

<!--modal pop up untuk detail id-->
<div class="modal fade" id="detail" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
        <div class="card" style="width: 18rem;">
              <ul class="list-group list-group-flush">
                <li class="list-group-item"><img :src="davatar" alt="orang"></li>
                <li class="list-group-item">{{ dname }}</li>
                <li class="list-group-item">{{ demail }}</li>
              </ul>
</div>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary">Save changes</button>
      </div>
    </div>
  </div>
</div>


<div class="modal fade" id="" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
        <div class="card" style="width: 18rem;">
              <ul class="list-group list-group-flush">
                <li class="list-group-item"><img :src="davatar" alt="orang"></li>
                <li class="list-group-item">{{ dname }}</li>
                <li class="list-group-item">{{ demail }}</li>
              </ul>
        </div>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary">Save changes</button>
      </div>
    </div>
  </div>
</div>

 
</template>

<script>

//axios merupakan metode untumendapatkan data dari data base dan harus di panggil setelah mounted
import axios from 'axios'

export default {
  name: 'App',
      data(){
        return{
          karyawan : [],
          name:'',
          email:'',
          Fname:'',
          Femail:'',
          avatar: null,
          dname: '',//dibuat berdasarkan this name yg ada pada get sebagai penampung
          demail: '',
          davatar: '',
          Judul: ''
        }
      },

      //fetch adalah metode untuk mendapatkan data dari database
      // fetch("https://reqres.in/api/users?page=2")
      //   .then(res => res.json())
      //   .then(res => console.log(res))


      //mounted juga metode untuk mendapatkan data dari data base,tapi harus menggunakan axios
      mounted(){
        axios.get("https://reqres.in/api/users?page=2")
        .then(res => (this.karyawan = res.data.data, console.log(res.data.data)))
      },
      methods:{
        submitForm(){
          let formData = new FormData(); // let formData adalah object yang kita buat sendiri untuk menampung data yg di kirim melalui form
          formData.append('name',this.name) // append adalah perintah menampung data ke object berdasarkan keynya seperti this.name
          formData.append('email',this.email)
          formData.append('avatar',this.avatar)

          // axios.post("https://reqres.in/api/users", formData,{
          //   headers:{
          //     'Content-Type': "multipart/form-data"
          //   }
          axios.post("https://reqres.in/api/users", formData,{
              headers:{
                'Content-Type': "multipart/form-data"
              }
            })
          .then(res => {
            console.log("berhasil",res)
             this.Fname = ''
             this.Femail = ''
             this.name = ''
             this.email =''
             document.querySelectorAll('input').forEach(e => {
              e.value = ''
             })

          })
          .catch(err => console.error("gagal",err))
        },
        hapus(bebas){
          axios.delete('https://reqres.in/api/users/'+bebas)
          .then(res => (console.log('berhasil'),console.log(res)))
          
        },

        ubahKaryawan(bebas, ...data){
          this.Judul = 'Ubah Karyawan'

          this.Fname = data[0]+' '+data[1]
          this.Femail = data[2]

          // axios.put('https://reqres.in/api/users/'+bebas, {
          //   name: this.name,
          //   email: this.emai
          // })
          // .then(res => (console.log('berhasil'),console.log(res)))
          // .catch(err => (console.log('gagal'),console.log(err)))
        },
        ViewKaryawan(bebas){
                          axios.get('https://reqres.in/api/users/'+bebas)
                            .then(res => {
                                          this.dname = res.data.data.first_name +' '+res.data.data.last_name
                                          this.davatar = res.data.data.avatar
                                          this.demail = res.data.data.email
                                        })
                            }
      },

}
</script>


