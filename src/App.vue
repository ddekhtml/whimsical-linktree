<script setup>
import { computed, onMounted, ref, watch } from 'vue'
import Data from './components/Data.vue'
import { supabase } from './utils/supabase'
import bgImage from './assets/image.png'
const search = ref('')
const length = ref(0)
const craft = ref([])

const isSearch = ref(false)
const isPagination = ref(1)

const lengthPerPagination = 10

const pagination = computed(() => {

  if (length.value === 0) {
    return 1
  }

  return Math.ceil(
    length.value / lengthPerPagination
  )

})

const nowDataPagination = computed(() => {

  const remaining =
    length.value -
    (
      (isPagination.value - 1) *
      lengthPerPagination
    )

  return Math.min(
    remaining,
    lengthPerPagination
  )

})
async function getCraft() {

  const from =
    (isPagination.value - 1) *
    lengthPerPagination

  const to =
    from + lengthPerPagination - 1

  let query = supabase
    .from('Craft')
    .select(`
      *,
      Product (*)
    `, {
      count: 'exact'
    })
  if (search.value.trim() !== '') {

    isSearch.value = true

    query = query.ilike(
      'name',
      `%${search.value}%`
    )

  } else {

    isSearch.value = false

  }
  query = query.range(from, to)

  const {
    data,
    error,
    count
  } = await query

  if (error) {

    console.log(error)

  } else {

    craft.value = data
    length.value = count

  }

}

const container = ref(null)

function changePage(page) {

  isPagination.value = page
  getCraft()

  container.value.scrollTo({
    top: 0,
    behavior: 'smooth'
  })

}

let timeout = null

watch(search, () => {

  clearTimeout(timeout)

  timeout = setTimeout(() => {

    isPagination.value = 1
    getCraft()

  }, 500)

})

onMounted(() => {

  getCraft()

})
</script>

<template>
  <div class="w-full min-h-screen bg-soft-pink flex justify-center overflow-hidden">
    <div
      class="w-107.5 h-screen overflow-y-auto scrollbar-none bg-cover bg-center bg-no-repeat"
      :style="{
    backgroundImage: `url(${bgImage})`
  }"
      ref="container"
    >
      <div class="w-full mb-30">
        
        <video src="/src/assets/510d4332199e43fa84044b09969b86f7.webm" autoplay loop muted></video>
        <div class="w-[90%] flex justify-center items-center gap-4 mx-auto mt-3 text-fushia">
  
          <div class="flex items-center gap-2 bg-button/10 px-4 py-2 rounded-full backdrop-blur-sm">
            <a href="https://www.instagram.com/ddekhtml/">
              <i class="pi pi-instagram"></i>
            </a>
            
          </div>

          <div class="flex items-center gap-2 bg-button/10 px-4 py-2 rounded-full backdrop-blur-sm">
            <a href="https://www.tiktok.com/@litlecorner">
              <i class="pi pi-tiktok"></i>
            </a>
          </div>
          <div class="flex items-center gap-2 bg-button/10 px-4 py-2 rounded-full backdrop-blur-sm">
            <a href="mailto:emailkamu@gmail.com?subject=Collaborate Litle Corner">
              <i class="pi pi-envelope"></i>
            </a>
          </div>

        </div>
        <div class="bg-button/20 text-fushia rounded-full py-2 pl-5 w-[90%] text-xl mx-auto mt-5 flex gap-2">
  
          <input
            type="text"
            placeholder="search"
            class="outline-none w-full bg-transparent"
            v-model="search"
          >
        </div>
        <div class="w-[90%] mx-auto mt-3 flex flex-col gap-3 ">
          <div v-if="craft.length===0 && isSearch===true && search.length!== 0" class="text-center text-fushia ">
            {{ search }} tidak ditemukan
          </div>
          <div v-for="n in craft" >
            
            <Data  :name="n.name" :tiktok="n.tiktok" :instagram="n.instagram" :product="n.Product"/> 
          </div>
        </div>
        <div class="flex w-[80%] mx-auto text-xl mt-5 justify-center gap-4 overflow-x-auto scrollbar-none">
          <div v-for="n in pagination" class="w-fit px-2 hover:cursor-pointer py-1 rounded-xl" :class="isPagination===n?'bg-button text-soft-pink':' bg-button/20 text-fushia '" @click="changePage(n)">
            {{ n }}
          </div>
          
        </div>

      </div>

    </div>

  </div>
</template>
