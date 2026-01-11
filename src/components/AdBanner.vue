<template>
  <div class="col-span-full w-full h-[120px] bg-gradient-to-r from-[#2c3e50] to-[#3498db] rounded-xl flex items-center justify-center relative overflow-hidden my-4 shadow-md group cursor-pointer">
    <ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-7373658997056234"
     data-ad-slot="4208171806"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
  </div>
</template>

<script setup>
import { onMounted } from 'vue';

// AdBanner component
onMounted(() => {
  try{
  if(window.adsbygoogle && this.$el){
    window.adsbygoogle.push({});
  };
  }
  catch(error){
    console.warn('Adsense Error', error)
  }
});
</script>

