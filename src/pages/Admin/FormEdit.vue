<template>
    <q-page padding>
        <div class="row q-mb-md col-gutter-md">
          <div class="col-md-12 col-xs-12 col-lg-12">
            <div class="row">
              <div class="col-auto">
                <div class="left blue"></div>
              </div>
              <div class="col">
                <q-banner inline-actions class="text-blue-grey-14">
                  <div class="text-h6">Edit TIKET</div>
                  <div>Edit Data TIKET</div>
                </q-banner>
              </div>
            </div>
          </div>
        </div>
        <q-card flat>
          <q-card-section class="row">
            <q-form
              @submit="onSubmit()"
              class="q-gutter-md col-md-6 col-xs-12"
            >
              <q-input
                v-model="form.namabus"
                filled
                label="Nama BUSS"
                :rules="[ val => val && val.length > 0 || 'Mohon Isi Nama BUSS']"
              />

              <q-input
                v-model="form.harga"
                filled
                label="Harga"
                :rules="[ val => val > 0 || 'Mohon Isi Harga']"
              />

              <q-input
                v-model="form.lokasitujuan"
                filled
                label="Lokasi Tujuan"
                :rules="[ val => val && val.length > 0 || 'Mohon Isi Lokasi Tujuan Anda']"
              />

              <q-select
                filled
                v-model="form.kelasbus"
                :options="optionKelasbus"
                label="Pilih Kelasbus"
              />
              <q-input
                v-model="form.deskripsi"
                filled
                label="Deskripsi"
                type="textarea"
              />

              <q-file accept=".jpg, image/*" color="teal" filled v-model="image" label="Upload Gambar">
                <template v-slot:prepend>
                  <q-icon name="cloud_upload" />
                </template>
              </q-file>

              <div>
                <q-btn label="Submit" type="submit" color="primary"/>
                <q-btn label="Reset" type="reset" color="primary" flat class="q-ml-sm" />
              </div>
            </q-form>
          </q-card-section>
        </q-card>
    </q-page>
</template>
<script>
export default {
  data () {
    return {
      form: {
        namabus: null,
        harga: 0,
        lokasitujuan: null,
        kelasbus: null,
        deskripsi: null
      },
      optionKelasbus: [
        'Eksecutive',
        'Bussines',
        'Economy',
        'RoyalClass'
      ],
      image: null
    }
  },
  created () {
    this.getData()
  },
  methods: {
    getData () {
      this.$axios.get(`ticket/getbyid/${this.$route.params.id}`)
        .then(res => {
          if (res.data.sukses) {
            this.form = res.data.data
          } else {
            this.$router.push({ name: 'dataTIKET' })
          }
        })
    },
    onSubmit () {
      const formData = new FormData()
      formData.append('image', this.image)
      formData.append('data', JSON.stringify(this.form))
      this.$axios.put(`ticket/edit/${this.$route.params.id}`, formData)
        .then(res => {
          if (res.data.sukses) {
            this.$showNotif(res.data.pesan, 'positive')
            this.$router.push({ name: 'dataTIKET' })
          } else {
            this.$showNotif(res.data.pesan, 'negative')
          }
        })
    }
  }
}
</script>
<style scoped>
.left{
  width: 5px;
  height: 100%;
  background-color:rgb(89, 251, 251);
}
</style>
