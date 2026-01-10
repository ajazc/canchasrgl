<template>
  <div class="bg-white/20 px-4 py-2 rounded-xl backdrop-blur-sm text-[#2c3e50] text-sm border border-white/10 transition-all duration-300 ease-in-out hover:bg-white/30" v-if="weather">
    <div class="flex items-center gap-3">
      <span class="text-[1.8rem]">{{ weatherCondition.icon }}</span>
      <div class="flex flex-col">
        <span class="text-xs opacity-90 uppercase tracking-wider font-semibold">Río Gallegos</span>
        <div class="flex items-center gap-2">
          <span class="text-lg font-bold">{{ Math.round(weather.temperature) }}°C</span>
          <span class="text-sm opacity-90 pl-2 border-l border-white/40">{{ weatherCondition.label }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue'

const weather = ref(null)

const weatherCodes = {
  0: { label: 'Despejado', icon: '☀️' },
  1: { label: 'Mayormente despejado', icon: '🌤️' },
  2: { label: 'Parcialmente nublado', icon: '🌤️' },
  3: { label: 'Nublado', icon: '☁️' },
  45: { label: 'Niebla', icon: '🌫️' },
  48: { label: 'Niebla con escarcha', icon: '🌫️' },
  51: { label: 'Llovizna ligera', icon: '🌧️' },
  53: { label: 'Llovizna moderada', icon: '🌧️' },
  55: { label: 'Llovizna intensa', icon: '🌧️' },
  61: { label: 'Lluvia ligera', icon: '🌧️' },
  63: { label: 'Lluvia moderada', icon: '🌧️' },
  65: { label: 'Lluvia intensa', icon: '🌧️' },
  71: { label: 'Nevada ligera', icon: '❄️' },
  73: { label: 'Nevada moderada', icon: '❄️' },
  75: { label: 'Nevada intensa', icon: '❄️' },
  95: { label: 'Tormenta', icon: '⚡' },
  96: { label: 'Tormenta con granizo ligero', icon: '⚡' },
  99: { label: 'Tormenta con granizo intenso', icon: '⚡' }
}

const weatherCondition = computed(() => {
  if (!weather.value) return { label: '', icon: '' }
  
  // Check for high wind speed (> 40 km/h)
  if (weather.value.windspeed > 40) {
    return { label: 'Ventoso', icon: '💨' }
  }

  const code = weather.value.weathercode
  return weatherCodes[code] || { label: 'Desconocido', icon: 'QmQ' }
})

const fetchWeather = async () => {
  try {
    const response = await fetch(
      'https://api.open-meteo.com/v1/forecast?latitude=-51.6226&longitude=-69.2181&current_weather=true'
    )
    const data = await response.json()
    weather.value = data.current_weather
  } catch (error) {
    console.error('Error fetching weather:', error)
  }
}

onMounted(() => {
  fetchWeather()
  setInterval(fetchWeather, 30 * 60 * 1000) // Update every 30 minutes
})
</script>
