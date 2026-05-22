<script setup>
import { ref, computed } from 'vue'

const search = ref('')
const currentPage = ref(1)

const sections = Array.from({ length: 30 }, (_, i) => ({
  id: i + 1
}))

const perPage = 10

const totalPage = computed(() => {
  return Math.ceil(sections.length / perPage)
})

const paginatedSections = computed(() => {
  const start = (currentPage.value - 1) * perPage
  return sections.slice(start, start + perPage)
})
</script>

<template>
  <div class="w-full min-h-screen bg-soft-pink flex justify-center overflow-hidden">
    
    <div class="relative w-[430px] h-screen overflow-hidden">

      <img 
        src="/src/assets/bg.png" 
        alt=""
        class="absolute inset-0 w-full h-full object-cover"
      >

      <div class="absolute top-55 left-1/2 -translate-x-1/2 z-10 w-[85%] bg-button/20 rounded-full pl-7 pr-3 py-2 text-xl flex items-center gap-4 text-fushia backdrop-blur-md">

        <input
          type="text"
          v-model="search"
          placeholder="search"
          class="outline-none w-full bg-transparent placeholder:text-fushia"
        >

        <button class="pi pi-search text-soft-pink bg-fushia rounded-full w-10 h-10 flex items-center justify-center text-sm shrink-0">
        </button>

      </div>


      <div class="absolute top-[300px] bottom-5 left-1/2 -translate-x-1/2 z-10 w-[85%] flex flex-col">
        <div class="flex-1 overflow-y-auto flex flex-col gap-5 pb-5 [&::-webkit-scrollbar]:hidden [-ms-overflow-style:none] [scrollbar-width:none]">
          <div 
            v-for="section in paginatedSections" 
            :key="section.id"
            class="rounded-[35px] bg-pink-200/55 backdrop-blur-md p-5"
          >
            <div class="flex items-center justify-between mb-4">

              <h2 class="text-pink-700 text-xl font-semibold">
                Ribbon Rosette
              </h2>

              <button class="bg-pink-400 text- text-white rounded-full px-4 py-1 text-sm">
                <span class="pi pi-tiktok font-yuji text-3xl"> video </span>
              </button>

            </div>

            <!-- HORIZONTAL SCROLL -->
            <div class="flex gap-3 overflow-x-auto pb-2 [&::-webkit-scrollbar]:hidden [-ms-overflow-style:none] [scrollbar-width:none]">

              <div
                v-for="n in 10"
                :key="n"
                class="min-w-[120px] h-[150px] rounded-[25px] bg-pink-300/70 flex items-center justify-center text-pink-700 font-bold flex-shrink-0"
              >
                {{ n }}
              </div>

            </div>

          </div>

        </div>
        <div class="flex justify-center gap-2 pt-2">

          <button
            v-for="page in totalPage"
            :key="page"
            @click="currentPage = page"
            class="w-10 h-10 rounded-2xl transition-all"
            :class="
              currentPage === page
              ? 'bg-pink-500 text-white scale-110'
              : 'bg-pink-200 text-pink-700'
            "
          >
            {{ page }}
          </button>

        </div>

      </div>

    </div>

  </div>
</template>