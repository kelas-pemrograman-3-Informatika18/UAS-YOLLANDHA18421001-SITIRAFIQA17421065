<template>
  <q-page padding>
    <div class="row q-mb-md col-gutter-md">
      <div class="col-md-12 col-xs-12 col-lg-12">
        <div class="row">
          <div class="col-autor">
            <div class="left blue"></div>
          </div>
          <div class="col">
            <q-banner inline-actions class="text-deep-orange-10">
              <div class="text-h6">Input Clothing</div>
              <div >Input Data Clothing</div>
            </q-banner>
          </div>
        </div>
      </div>
    </div>

    <q-card flat>
      <q-card-section class="row">
        <q-form
        @submit="onSubmit()"
            class="q-col-gutter-md q-col-lg-6 col-md-6 col-xs-12"
            >
            <q-input
                filled
                v-model="form.namaBaju"
                label="Nama Baju"
                :rules="[ val => val && val.length > 0 || 'Masukkan Nama Baju']"
            />

             <q-input
              filled
              v-model="form.harga"
              label="Harga"
              type="number"
              :rules="[ val => val && val.length > 0 || 'Masukkan Harga']"
            />

            <q-input
              filled
              v-model="form.warna"
              :options="optionWarna"
              label="Pilih Warna Baju"
              :rules="[ val => val && val.length > 0 || 'Pilihlah Warna Baju']"
            />

            <q-select
              filled
              v-model="form.ukuran"
              :options="optionUkuran"
              label="Pilih Ukuran"
              :rules="[ val => val && val.length > 0 || 'Pilihlah Ukuran']"
            />

            <div class="flex">
              Pilih Rating
              <q-rating
              class="q-ml-md"
              v-model="form.rating"
              size="2em"
              :max="5"
              color="deep-orange-10"
            />
            </div>

            <q-input
              v-model="form.deskripsi"
              filled
              type="textarea"
              label="Deskripsi"
              :rules="[ val => val && val.length > 0 || 'Masukan Deskripsi']"
            />

            <q-file accept=".jpg, image/*" color="light-green" filled v-model="image" label="Upload Gambar">
              <template v-slot:prepend>
                <q-icon name="cloud_upload" />
              </template>
            </q-file>

            <div>
                <q-btn label="Submit" type="submit" color="deep-orange-10"/>
                <q-btn label="Reset" type="reset" color="deep-orange-14" flat class="q-ml-sm" />
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
        namaBaju: null,
        harga: 0,
        warna: null,
        ukuran: null,
        rating: 0,
        deskripsi: null
      },
      optionWarna: [
        'merah',
        'biru',
        'hijau',
        'coklat',
        'kuning',
        'putih'
      ],
      optionUkuran: [
        'xs',
        's',
        'm',
        'l',
        'xl',
        'xxl'
      ],
      image: null
    }
  },
  methods: {
    onSubmit () {
      const formData = new FormData()
      formData.append('image', this.image)
      formData.append('data', JSON.stringify(this.form))
      this.$axios.post('baju/insert', formData)
        .then(res => {
          if (res.data.sukses) {
            this.$showNotif(res.data.pesan, 'positive')
            this.$router.push({ name: 'dataBaju' })
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
  width: 4px;
  height: 100%;
  background-color: rgb(24, 175, 4);
}
</style>
