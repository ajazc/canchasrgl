<template>
  
    <ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-7373658997056234"
     data-ad-slot="4208171806"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

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

