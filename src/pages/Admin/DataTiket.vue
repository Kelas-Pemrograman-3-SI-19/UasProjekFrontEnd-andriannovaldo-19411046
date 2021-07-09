<template>
  <q-pag padding>
    <div class="row q-mb-md col-gutter-md">
      <div class="col-md-12 col-xs-12 col-lg-12">
        <div class="row">
          <div class="col-auto">
            <div class="left blue"></div>
          </div>
          <div class="col">
            <q-banner inline-actions class="text-blue-grey-14">
              <div class="text-h6">Data TIKET</div>
              <div>Data Penjualan Tiket</div>
            </q-banner>
          </div>
        </div>
      </div>
    </div>
    <q-card>
      <q-table
        :rows="data"
        :columns="columns"
        row-key="name"
      >
        <template v-slot:body="props">
          <q-tr :props="props">
            <q-td key="namabus" :props="props">
                {{ props.row.namabus }}
              </q-td>
            <q-td key="harga" :props="props">
              Rp {{ props.row.harga }},-
            </q-td>
            <q-td key="lokasitujuan" :props="props">
              {{ props.row.lokasitujuan }}
            </q-td>
            <q-td key="kelasbus" :props="props">
              {{ props.row.kelasbus }}
            </q-td>
            <q-td key="deskripsi" :props="props">
                <div class="ellipsis" style="max-width: 100px;">
                    {{ props.row.deskripsi }}
                </div>
            </q-td>
            <q-td key="gambar" :props="props">
              <q-img
                  :src="`http://localhost:5000/${props.row.image}`"
                  spinner-color="blue"
                  style="height: 120px; max-width: 85px"
              />
            </q-td>
            <q-td key="aksi" :props="props">
              <div class="row q-gutter-md">
                <q-btn :to="{ name:'formEditTIKET', params: { id: props.row._id } }" label="Edit" icon="edit" color="warning" unelevated/>
                <q-btn @click="deleteTicket(props.row._id)" label="Hapus" icon="delete" color="negative" unelevated/>
              </div>
            </q-td>
          </q-tr>
        </template>
      </q-table>
    </q-card>
  </q-pag>
</template>

<script>
export default {
  name: 'PageIndex',
  data () {
    return {
      columns: [
        { name: 'namabus', align: 'left', label: 'Nama BUSS', field: 'namabus', sortable: true },
        { name: 'harga', align: 'left', label: 'Harga', field: 'harga', sortable: true },
        { name: 'lokasitujuan', align: 'left', label: 'Lokasi Tujuan', field: 'lokasitujuan', sortable: true },
        { name: 'kelasbus', align: 'left', label: 'Kelas BUSS', field: 'kelasbus', sortable: true },
        { name: 'deskripsi', align: 'left', label: 'Deskripsi', field: 'deskripsi', sortable: true },
        { name: 'gambar', align: 'left', label: 'Gambar', field: 'gambar' },
        { name: 'aksi', align: 'left', label: 'Aksi', field: 'aksi' }
      ],
      data: []
    }
  },
  created () {
    this.getData()
  },
  methods: {
    getData () {
      this.$axios.get('ticket/getall')
        .then((res) => {
          if (res.data.sukses) {
            this.data = res.data.data
          } else {
            this.showNotif(res.data.pesan, 'negative')
          }
        })
    },
    deleteTicket (id) {
      this.$q.dialog({
        title: 'Konfirmasi',
        message: 'Apakah Anda Yakin?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.$axios.delete(`ticket/delete/${id}`)
          .then(res => {
            if (res.data.sukses) {
              this.$showNotif(res.data.pesan, 'positive')
              this.getData()
            } else {
              this.$showNotif(res.data.pesan, 'negative')
            }
          })
      })
    }
  }
}
</script>
<style scoped>
.left {
  width: 5px;
  height: 100%;
  background: rgb(3, 196, 180);
}
</style>
