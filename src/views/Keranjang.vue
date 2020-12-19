<template>
  <div class="keranjang">
    <Navbar :updateKeranjang="keranjangs"/>
    <div class="container">
      <div class="row mt-5">
        <div class="col">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <router-link to="/">Home</router-link>
              </li>
              <li class="breadcrumb-item">
                <router-link to="/foods">Foods</router-link>
              </li>
              <li class="breadcrumb-item active" aria-current="page">
                Keranjang
              </li>
            </ol>
          </nav>
        </div>
      </div>

      <div class="row">
        <div class="col">
          <h2>Keranjang <strong>Saya</strong></h2>
          <div class="table-responsive mt-3">
            <table class="table">
              <thead>
                <tr>
                  <th scope="col">#</th>
                  <th scope="col">Foto</th>
                  <th scope="col">Makanan</th>
                  <th scope="col">Keterangan</th>
                  <th scope="col">Jumlah</th>
                  <th scope="col">Harga</th>
                  <th scope="col">Total Harga</th>
                  <th scope="col">Hapus</th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="(keranjang, index) in keranjangs"
                  :key="keranjang.id"
                >
                  <th>{{ index + 1 }}</th>
                  <td>
                    <img
                      class="img-fluid shadow"
                      :src="'../assets/images/' + keranjang.products.gambar"
                      alt="Card image cap" width="250"
                    />
                  </td>
                  <td>{{keranjang.products.nama}}</td>
                  <td>{{keranjang.keterangan ? keranjang.keterangan: "-"}}</td>
                  <td>{{keranjang.jumlah_pemesanan}}</td>
                  <td>{{keranjang.products.harga}}</td>
                  <td><strong>Rp. {{keranjang.products.harga*keranjang.jumlah_pemesanan}}</strong></td>
                  <td align="center" class="text-danger">
                    <b-icon-trash @click="hapusKeranjang(keranjang.id)"></b-icon-trash>
                  </td>
                </tr>
                <tr>
                  <td colspan="6" align="right">
                    <strong>Total Harga :</strong>
                  </td>
                  <td>
                    <strong>Rp. {{totalHarga}}</strong>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import axios from "axios";

export default {
  name: "Keranjang",
  components: {
    Navbar,
  },
  data() {
    return {
      keranjangs: [],
    };
  },
  methods: {
    setKeranjangs(data) {
      this.keranjangs = data;
    },
    hapusKeranjang(id){
      axios
      .delete("http://localhost:3000/keranjangs/"+id)
      .then(() => {
        this.$toast.error("Sukses hapus keranjang", {
          type: "error",
          position: "top-right",
          duration: 3000,
          dismissible: true,
        })
        
        axios
        .get("http://localhost:3000/keranjangs")
        .then((response) => this.setKeranjangs(response.data))
        .catch(function (error) {
          // handle error
          console.log("Gagal : ", error);
        });
      }
      
        
      )
      .catch(function (error) {
        // handle error
        console.log("Gagal : ", error);
      });

      
    }
  },
  mounted() {
    axios
      .get("http://localhost:3000/keranjangs")
      .then((response) => this.setKeranjangs(response.data))
      .catch(function (error) {
        // handle error
        console.log("Gagal : ", error);
      });
  },
  computed: {
    totalHarga(){
      return this.keranjangs.reduce(function(items, data){
        return items+(data.products.harga* data.jumlah_pemesanan)
      }, 0)
    }
  }
};
</script>