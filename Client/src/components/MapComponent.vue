<template>
  <div>
    <div id="mapContainer"/>
  </div>
</template>

<script>
import { DefineComponent, onBeforeMount, onMounted } from 'vue';
import 'leaflet/dist/leaflet.css'
import L from 'Leaflet'
export default {
  props:{
    markers:{
      type: array,
      required: false,
      default:()=> []
    }
  },
  setup(props){
    let map = null
    
    onMounted(()=>{
      createMapLayer()
    })

    onBeforeMount(()=>{
      if(map){
        map.remove()
      }
    })


    const createMapLayer = () => {
      map = L.map('mapContainer').setView([], 5)
      L.TileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
      }).addTo(map)

      if(props.markers.length){
        setMarkers()
      }
    }

    const setMarkers = () => {
      props.markers.map((marker) =>{
        return L.marker([marker.latitude, marker.longitude]).addTo(map).bindPopup(marker.descricao)
      })
    }
  }
}
</script>

<style>

</style>
