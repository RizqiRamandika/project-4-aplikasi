<template>
  <div id="a">
    <br />
    <div
      style="
        margin-left: 150px;
        margin-right: 150px;
        background-color: yellow;
        border: 2px dashed black;
        border-radius: 20px;
      "
    >
      <marquee><h2>SELAMAT DATANG DI APLIKASI TOKO BUKU</h2></marquee>
    </div>
    <br />
    <form
      style="margin-left: 180px; margin-right: 180px"
      @submit.prevent="add"
      action="login.php"
      method="POST"
    >
      <fieldset style="border-radius: 20px; border: 2px solid black">
        <legend style="text-align: center">
          <h1
            style="
              background-color: yellow;
              padding: 10px;
              border: 2px solid black;
              border-radius: 10px;
            "
          >
            FORM PEMINJAMAN BUKU
          </h1>
        </legend>
        <center>
          <p style="padding-left: 10px">
            <label><b>Nama Siswa</b></label
            ><br />
            <input
              placeholder="Nama Siswa..."
              style="
                width: 295px;
                height: 30px;
                margin-top: 5px;
                background-color: lightblue;
              "
              required
              type="text"
              v-model="form.name"
            />
          </p>
          <p style="padding-left: 10px">
            <label><b>Judul Buku</b></label
            ><br />
            <input
              placeholder="Judul Buku..."
              style="
                width: 295px;
                height: 30px;
                margin-top: 5px;
                background-color: lightblue;
              "
              required
              type="text"
              v-model="form.pengarang"
            />
          </p>
          <p style="padding-left: 10px">
            <label><b>Tanggal Pinjam</b></label
            ><br />
            <input
              placeholder="Tanggal Pinjam..."
              style="
                width: 295px;
                height: 30px;
                margin-top: 5px;
                background-color: lightblue;
              "
              required
              type="text"
              v-model="form.terbit"
            />
          </p>
          <p style="padding-left: 10px">
            <label><b>Tanggal Pengembalian</b></label
            ><br />
            <input
              placeholder="Tanggal Pengembalian..."
              style="
                width: 295px;
                height: 30px;
                margin-top: 5px;
                background-color: lightblue;
              "
              required
              type="text"
              v-model="form.kategori"
            />
          </p>
          <br />
          <button
            style="
              margin-left: 10px;
              width: 300px;
              height: 40px;
              font-size: 20px;
              border-radius: 10px;
              background-color: lightblue;
            "
            type="submit"
            v-show="!updateSubmit"
          >
            Add Pinjaman
          </button>
          <button
            style="
              margin-left: 10px;
              width: 300px;
              height: 40px;
              font-size: 20px;
              border-radius: 10px;
              background-color: lightblue;
            "
            type="button"
            v-show="updateSubmit"
            @click="update(form)"
          >
            Perpanjang
          </button>
        </center>
        <br />
      </fieldset>
    </form>
    <br />
    <center>
      <fieldset
        style="
          border-radius: 20px;
          border: 2px solid black;
          margin-left: 50px;
          margin-right: 50px;
        "
      >
        <legend style="text-align: center">
          <h1
            style="
              background-color: yellow;
              padding: 10px;
              border: 2px solid black;
              border-radius: 10px;
            "
          >
            TABEL DAFTAR BUKU
          </h1>
        </legend>
        <form style="margin-left: 520px">
          <input
            style="border: none; font-size: 20px; background-color: lime"
            class="button"
            type="button"
            value="Search"
          />
          <input
            style="margin-left: 5px; background-color: yellow; height: 20px"
            class="search"
            type="text"
            required
          />
        </form>
        <br />
        <table
          style="
            padding: 5px;
            border-color: black;
            background-color: yellow;
            border-radius: 10px;
          "
        >
          <tr style="padding: 10px; background-color: black; color: white">
            <th style="padding: 5px; border-radius: 5px">No</th>
            <th style="padding: 5px; border-radius: 5px">Nama Siswa</th>
            <th style="padding: 5px; border-radius: 5px">Judul Buku</th>
            <th style="padding: 5px; border-radius: 5px">Tanggal Pinjam</th>
            <th style="padding: 5px; border-radius: 5px">
              Tanggal Pengembalian
            </th>
            <th style="padding: 5px; border-radius: 5px">Aksi</th>
          </tr>
          <tr style="padding: 10px" v-for="(user, index) in users" :key="index">
            <th style="padding: 5px">{{ ++index }}</th>
            <th style="padding: 5px">{{ user.name }}</th>
            <th style="padding: 5px">{{ user.pengarang }}</th>
            <th style="padding: 5px">{{ user.terbit }}</th>
            <th style="padding: 5px">{{ user.kategori }}</th>
            <th style="padding: 5px">
              {{ user.action }}
              <button
                style="background-color: green; color: white; padding: 5px"
                @click="edit(user)"
              >
                Perpanjang
              </button>
              ||
              <button
                style="
                  background-color: rgb(255, 0, 0);
                  color: white;
                  padding: 5px;
                "
                @click="del(user)"
              >
                Kembalikan
              </button>
            </th>
          </tr>
        </table>
        <br />
      </fieldset>
      <br /><br />
    </center>
  </div>
</template>

<script>
/* eslint-disable */
import axios from "axios";
export default {
  data() {
    return {
      form: {
        id: "",
        name: "",
        pengarang: "",
        terbit: "",
        kategori: "",
      },
      users: "",
      updateSubmit: false,
    };
  },
  mounted() {
    this.load();
  },
  methods: {
    load() {
      axios
        .get("http://localhost:3000/users")
        .then((res) => {
          this.users = res.data; //respon dari rest api dimasukan ke users
        })
        .catch((err) => {
          console.log(err);
        });
    },
    add() {
      axios.post("http://localhost:3000/users/", this.form).then((res) => {
        this.load();
        this.form.id = "";
        this.form.name = "";
        this.form.pengarang = "";
        this.form.terbit = "";
        this.form.kategori = "";
      });
    },
    edit(user) {
      this.updateSubmit = true;
      this.form.id = user.id;
      this.form.name = user.name;
      this.form.pengarang = user.pengarang;
      this.form.terbit = user.terbit;
      this.form.kategori = user.kategori;
    },
    update(form) {
      return axios
        .put("http://localhost:3000/users/" + form.id, {
          name: this.form.name,
          pengarang: this.form.pengarang,
          terbit: this.form.terbit,
          kategori: this.form.kategori,
        })
        .then((res) => {
          this.load();
          this.form.id = "";
          this.form.name = "";
          this.form.pengarang = "";
          this.form.terbit = "";
          this.form.kategori = "";
          this.updateSubmit = false;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    del(user) {
      axios.delete("http://localhost:3000/users/" + user.id).then((res) => {
        this.load();
        let index = this.users.indexOf(form.name);
        this.users.splice(index, 1);
      });
    },
  },
};
</script>

<style>
#a {
  border-radius: 25px;
  font-family: Arial;
  background-color: lime;
  margin-left: 200px;
  margin-right: 200px;
  margin-top: 30px;
  margin-bottom: 30px;
}
#b {
  display: flex;
}
</style>