<script setup>
const props=defineProps(
  {
    name: String, 
    tiktok: String, 
    instagram: String, 
    product : Array
  },
)
</script>
<template>
  <div class="w-full bg-button/20 min-h-40 rounded-2xl text-fushia p-3">
    <div class="flex justify-between">
      <div class="text-xl text-wrap">{{props.name}}</div>
      <div class="flex gap-3">
        
        <a :href="props.tiktok" class="hover:cursor-pointer" v-if="props.tiktok"><i class="pi pi-tiktok"></i></a>
        <a :href="props.instagram" class="hover:cursor-pointer" v-if="props.instagram"><i class="pi pi-instagram"></i></a>
      </div>
    </div>
    <hr>
   <div class="flex h-full gap-5 w-full overflow-x-auto scrollbar-none  px-2">
    <div 
      v-for="n in props.product" 
      :key="n"
      class="shrink-0"
    >

      <a class=" my-3 flex flex-col gap-2" :href="n.link">
        <img :src="n.image" alt="" class="h-30 w-25 object-cover border-3 border-soft-pink rounded-2xl">
        <div class="text-center">
          {{n.description}}
        </div>
      </a>
    </div>

  </div>
  </div>
</template>