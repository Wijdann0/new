<script setup>
const supabase = useSupabaseClient()

const route = useRoute()
const buku = ref([])

const getBookById = async () => {
  const { data, error } = await supabase.from('Buku').select(`*, kategori(*)`)
    .eq('id', route.params.id)
  if (data) buku.value = data[0]
}

onMounted(() => {
  getBookById()
})
</script>

<template>
  <div class="container">
    <div class="row mt-4 d-flex justify-content-evenly">
      <div class="col-lg-4 p-0">
        <div class="card">
          <div class="card-body p-0">
            <img :src="buku.cover" width="220px" height="340px" class="cover" alt="cover1">
          </div>
        </div>
      </div>
      <div class="col-4 p-0">
        <p>Judul : {{ buku.judul }}</p>
        <p>Penulis : {{ buku.penulis }}</p>
        <p>Penerbit : {{ buku.penerbit }}</p>
        <p>Tahun : {{ buku.tahun_terbit }}</p>
      </div>
    </div>
    <div class="row mt-5">
      <h2>Sinopsis</h2>
      <p class="mt-3">{{ buku.deskripsi }}</p>
    </div>

    <nuxt-link to="/buku">
      <button type="button" class="btn btn-dark mt-5 mb-5">Kembali</button>
    </nuxt-link>
  </div>
</template>


<style scoped>
.cover {
  width: 255px;
  height: 370px;
  box-shadow: 1px 10px 50px rgb(0, 0, 0, .5);
}

.card {
  border: none !important;
}

.card-body {
  width: auto;
}
</style>
