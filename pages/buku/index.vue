<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center my-4">Cari Buku</h2>
        <div class="my-3">
          <form @submit.prevent="getBooks">
            <input v-model="keyword" type="search" class="form-control rounded-5" placeholder="Mau baca apa hari ini?">
          </form>
        </div>
        <div class="my-3 text-muted fs-6">menampilkan {{ books.length }} dari {{ jumlah }} </div>
        <div class="row">
          <div v-for="(book, i) in books" :key="i" class="col-lg-2">
            <nuxt-link :to="`/buku/${book.id}`">
              <div class="card mb-3 ">
                <div class="card-header" style="height: 250px;">
                  <img :src="book.cover" :alt="book.judul">
                </div>
                <div class="card-body">
                  <h6>{{ book.judul }}</h6>
                </div>
              </div>
            </nuxt-link>
          </div>
        </div>
      </div>
    </div>
    <nuxt-link to="/">
      <button type="button" class="btn btn-dark">Kembali</button>
    </nuxt-link>
  </div>
</template>

<style scoped>
.card-body {
  padding: 0;
}

.cover {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: 0 30;
}

button {
  width: 100px;
}

img {
  width: 100%;
}
</style>


<script setup>
const supabase = useSupabaseClient()

const books = ref([])
const jumlah = ref(0)
const keyword = ref('')

const getBooks = async () => {
  const { data, error } = await supabase.from('Buku').select(`*, kategori(*)`)
    .ilike('judul', `%${keyword.value}%`)
  if (data) books.value = data
}

const totalBuku = async () => {
  const { data, count } = await supabase.from('Buku').select("*", { count: 'exact' })
  if (data) jumlah.value = count
}

onMounted(() => {
  getBooks()
  totalBuku()
})
</script>