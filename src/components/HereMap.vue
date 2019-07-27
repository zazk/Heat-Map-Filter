<template>
  <div>
    <div id="heatmap-maplima" v-once />
    <div class="preload" style="display: none">
      <div class="imgpreload">
        <img src="static/preload.gif" alt="preload" />
      </div>
    </div>
    <div class="map--etiqueta">
      <span>Mostrando: {{ total }} Puntos classification:{{classification}} group:{{group}}</span>
    </div>
  </div>
</template>

<script>
import L from 'leaflet'
import HeatmapOverlay from '@/scripts/leaflet-heatmap'

import records from '@/data/records'
// --
const lines = records.split('\n')
const points = lines.map(record => {
  const [, , , , latitud, longitud, , , , tipo, classification, size, , , , , group] = record.split(',')
  console.log('Group', group)
  return { latitud, longitud, tipo, classification, size, group }
})
const headers = points.splice(0, 1)
// Remove, just for reading
const [pointExample] = points
// rememnet both are in strings, review if you need int/long numbers
const { latitud, longitud } = pointExample

const cfg = {
  // radius should be small ONLY if scaleRadius is true (or small radius is intended)
  // if scaleRadius is false it will be the constant radius used in pixels
  radius: 12,
  maxOpacity: 0.8,
  // scales the radius based on map zoom
  scaleRadius: false,
  // if set to false the heatmap uses the global maximum for colorization
  // if activated: uses the data maximum within the current map boundaries
  //   (there will always be a red spot with useLocalExtremas true)
  useLocalExtrema: true,
  // which field name in your data represents the latitude - default "lat"
  latField: 'latitud',
  // which field name in your data represents the longitude - default "lng"
  lngField: 'longitud',
  // which field name in your data represents the data value - default "value"
  valueField: 'count'
}
//
const heatmapLayer = new HeatmapOverlay(cfg)

export default {
  props: ['classification', 'group'],
  data: () => ({
    total: points.length,
    points: points
  }),
  mounted() {
    this.createMap()
  },
  beforeUpdate() {
    let data = []
    const filtered = {
      max: 8,
      data: []
    }
    const classification = this.classification
    const group = this.group
    if (classification != 'Todos' || group != 'Todos') {
      data = points.filter(p => {
        return (classification != 'Todos' && p.classification == classification) || (group != 'Todos' && p.group == group)
      })
      filtered.data = data
    } else {
      filtered.data = points
    }
    heatmapLayer.setData(filtered)
  },
  methods: {
    createMap() {
      const data = {
        max: 8,
        data: this.points
      }
      const baseLayer = L.tileLayer('http://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '...',
        maxZoom: 18
      })

      const map = new L.Map('heatmap-maplima', {
        center: new L.LatLng(latitud, longitud),
        zoom: 15,
        layers: [baseLayer, heatmapLayer]
      })
      heatmapLayer.setData(data)
    }
  }
}
</script>

<style lang="css" scoped>
.map--etiqueta {
  position: absolute;
  top: 10px;
  z-index: 600;
  right: 0;
  background: #fff;
  height: 40px;
  font-size: 14px;
  line-height: 40px;
  border-radius: 6px 0px 0px 6px;
  color: #797979;
  padding: 0px 10px 0px 30px;
  text-align: right;
  font-weight: bold;
  box-shadow: rgba(0, 0, 0, 0.3) 0px 1px 4px -1px;
}
#heatmap-maplima {
  position: absolute;
  top: 0;
  left: 0;
  height: calc(100vh - 60px);
  width: 100%;
  display: block;
}
.imgpreload {
  display: -webkit-flex;
  display: -moz-flex;
  display: -ms-flex;
  display: -o-flex;
  display: flex;
  -ms-align-items: center;
  align-items: center;
  justify-content: center;
  height: 100%;
}
.preload {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 200;
  margin: auto;
  background: #fff;
  display: -webkit-flex;
  display: -moz-flex;
  display: -ms-flex;
  display: -o-flex;
  display: flex;
  -ms-align-items: center;
  align-items: center;
  justify-content: center;
}
</style>
