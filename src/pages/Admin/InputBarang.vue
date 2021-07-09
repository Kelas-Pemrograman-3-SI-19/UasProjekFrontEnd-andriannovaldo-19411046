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
                <div class="text-h6">Input TIKET</div>
                <div>Input Data TIKET Baru</div>
              </q-banner>
           </div>
         </div>
      </div>
    </div>
     <q-card flat>
        <q-card-section class="row">
         <q-form
         @submit="onSubmit()"
            class="q-col-gutter-md q-col-lg-6 col-md-6 col-ms-12"
            >
            <q-input
                filled
                v-model="form.namabus"
                label="Nama BUSS "
                :rules="[ val => val && val.length > 0 || 'Mohon Isi NamaBuss Anda']"
            />

            <q-input
                filled
                v-model="form.harga"
                type="number"
                label="Harga "
                :rules="[ val => val > 0 || 'Mohon Isi Harga']"
            />

            <q-input
                filled
                v-model="form.lokasitujuan"
                label="Lokasi Tujuan "
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
                type="textarea"
                label="Deskripsi"
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
  methods: {
    onSubmit () {
      const formData = new FormData()
      formData.append('image', this.image)
      formData.append('data', JSON.stringify(this.form))
      this.$axios.post('/ticket/insert', formData)
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
.left {
  width: 5px;
  height: 100%;
  background-color: aqua;
}
</style>
