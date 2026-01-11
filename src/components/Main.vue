<template>
  <main class="flex-1 py-8 bg-[#f8f9fa]">
    <div class="max-w-[1200px] mx-auto px-8">
      <div class="flex justify-center mb-8">
        <WeatherWidget />
      </div>
      <section class="text-center mb-12">
        <h2 class="text-[2.5rem] text-[#2c3e50] mb-4">Tu Próximo Partido Comienza Acá</h2>
        <p class="text-[1.2rem] text-[#666]">Descubrí las mejores canchas de Río Gallegos y viví la pasión del fútbol al máximo.</p>
      </section>

      <section class="canchas-list">
        <h3 class="text-[2rem] text-[#2c3e50] mb-8 text-center">Canchas Disponibles</h3>
        <div class="grid grid-cols-[repeat(auto-fit,minmax(280px,1fr))] gap-4 px-2">
          <template v-for="(item, index) in itemsWithAds" :key="index">
            <!-- Cancha Card -->
            <div v-if="item.type === 'cancha'" class="bg-white rounded-xl overflow-hidden shadow-[0_4px_15px_rgba(0,0,0,0.1)] transition-transform duration-300 hover:-translate-y-[5px]">
              <img :src="item.data.imagen" :alt="item.data.nombre" class="w-full h-[160px] object-cover">
              <div class="p-4">
                <h4 class="text-[1.1rem] text-[#2c3e50] mb-[0.3rem]">{{ item.data.nombre }}</h4>
                <p class="text-[#666] my-[0.2rem] text-[0.9rem]">📍 {{ item.data.ubicacion }}</p>
                <p class="text-[#666] my-[0.2rem] text-[0.9rem]">⚽ {{ item.data.tipo }}</p>
                <a :href="`tel:${item.data.telefono}`" class="inline-flex items-center gap-2 text-[1rem] font-bold text-white bg-[#27ae60] hover:bg-[#219150] px-3 py-1.5 rounded-lg my-[0.3rem] transition-colors no-underline w-fit">
                  <span>📞</span>
                  {{ item.data.telefono }}
                </a>
                <div class="flex flex-wrap gap-[0.4rem] my-[0.5rem]">
                  <span v-for="caracteristica in item.data.caracteristicas" :key="caracteristica" class="bg-[#e3f2fd] text-[#1976d2] px-[0.6rem] py-[0.2rem] rounded-[20px] text-[0.75rem]">
                    {{ caracteristica }}
                  </span>
                </div>
              </div>
            </div>
            
            <!-- Ad Banner -->
            <AdBanner v-else-if="item.type === 'ad'" />
          </template>
        </div>
      </section>
    </div>
  </main>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import AdBanner from './AdBanner.vue'
import WeatherWidget from './WeatherWidget.vue'

// Importar imágen
import laGambetaImg from '../assets/La Gambeta.png'
import talleresImg from '../assets/Talleres.png'
import fairplayImg from '../assets/Fairplay.png'
import diasImg from '../assets/Dias.png'
import hispanoImg from '../assets/Hispano.png'
import yatelImg from '../assets/Yatel.png'
import laCurtiembreImg from '../assets/Curtiembre.png'
import lusaImg from '../assets/Lusa.png'

const canchas = ref([
  {
    id: 1,
    nombre: 'La Gambeta Rg',
    ubicacion: 'Pres. Dr. Raúl R. Alfonsín 237',
    tipo: 'Fútbol 5, Futbol 6',
    telefono: '2966 58-8195' ,
    imagen: laGambetaImg,
    caracteristicas: ['Césped sintético','Techo', 'Iluminación', 'Vestuarios']
  },
  {
    id: 2,
    nombre: 'Talleres',
    ubicacion: 'Mariano Moreno 157',
    tipo: 'Fútbol 7, Fútbol 6',
    telefono: '2966 44-7975',
    imagen: talleresImg,
    caracteristicas: ['Césped sintético', 'Iluminación', 'Estacionamiento',  'Cancha adentro']
  },
  {
    id: 3,
    nombre: 'Fairplay',
    ubicacion: 'Av. Parque Industrial 801-899',
    tipo: 'Fútbol 5, Fútbol 6',
    telefono: '2966 53-4791',
    imagen: fairplayImg,  
    caracteristicas: ['Techo', 'Iluminación', 'Estacionamiento']
  },
  {
    id: 4,
    nombre: 'Polideportivo Diaz',
    ubicacion: 'Juan Bark y Río Grande',
    tipo: 'Fútbol 5',
    telefono: '2966 70-1870',
    imagen: diasImg,
    caracteristicas: ['Césped sintético','Techo', 'Estacionamiento']
  },
  {
    id: 5,
    nombre: 'Hispano',
    ubicacion: 'Bartolomé Mitre 400-498',
    tipo: 'Fútbol 11, Fútbol 8',
    telefono: '2966 46-7395',
    imagen: hispanoImg,
    caracteristicas: ['Césped sintético','Iluminación', 'Estacionamiento']
  },
  {
    id: 6,
    nombre: 'Polideportivo Yatel  ',
    ubicacion: 'Av. Parque Industrial 645',
    tipo: 'Fútbol 5, Fútbol 6',
    telefono: '2966 52-5761',
    imagen: yatelImg,
    caracteristicas: ['Césped sintético', 'Iluminación', 'Estacionamiento', 'Cancha adentro']
  },
  {
    id: 7,
    nombre: 'La Curtiembre',
    ubicacion: 'Facundo Quiroga 99-199',
    tipo: 'Fútbol 5, Fútbol 6',
    telefono: '2966 41-2650',
    imagen: laCurtiembreImg,
    caracteristicas: ['Césped sintético', 'Iluminación', 'Estacionamiento', 'Cancha adentro']
  },
  {
    id: 8,
    nombre: 'Lusa futbol',
    ubicacion: 'Avenida Asturias Nro°730',
    tipo: 'Fútbol 5, Futbol 6',
    telefono: '2966 72-9823',
    imagen: lusaImg,
    caracteristicas: ['Césped sintético', 'Iluminación', 'Estacionamiento', 'Cancha adentro']
  }
])

const itemsWithAds = computed(() => {
  const items = []
  canchas.value.forEach((cancha, index) => {
    items.push({ type: 'cancha', data: cancha })
    // Add banner after every 3 items (approx 1 row on desktop)
    // Adjust this number '3' based on your grid layout preference
    if ((index + 1) % 3 === 0 && index !== canchas.value.length - 1) {
      items.push({ type: 'ad', id: `ad-${index}` })
    }
  })
  return items
})
onMounted(() => {
  //funion para mezclar las canchas y que aparezcan en orden aleatorio
  canchas.value.sort(() => Math.random() - 0.5) //que devuelve ?
})
</script>