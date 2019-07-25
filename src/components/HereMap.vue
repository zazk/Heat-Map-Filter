<template>
  <div>
    <div id="heatmap-maplima" />
    <div class="preload" style="display: none">
      <div class="imgpreload">
        <img src="static/preload.gif" alt="preload" />
      </div>
    </div>
    <div class="map--etiqueta">
      <span>Mostrando: {{ total }} Puntos</span>
    </div>
  </div>
</template>

<script>
const API_KEY = '9S-Qq8kiAq4g1KRquBAyDFAH64crVZ59IpW5MrGcy9c'
const lines = records.split('\n')
const points = lines.map(record => record.split(','))
const headers = points.splice(0, 1)
// Remove, just for reading
const [pointExample] = points
// rememnet both are in strings, review if you need int/long numbers
const [, , , , latitud, longitud] = pointExample
// --
import records from '@/data/records'

export default {
  props: ['total'],
  mounted() {
    console.log('points', points, 'headers', headers)
    console.log(
      'point example latitud',
      latitud,
      'point example longitud',
      longitud
    )
    this.createMap()
  },
  methods: {
    createMap() {
      let maptypes = new H.service.Platform({
        apikey: API_KEY
      }).createDefaultLayers()

      return new H.Map(
        document.getElementById('heatmap-maplima'),
        maptypes.vector.normal.map,
        {
          zoom: 10,
          center: { lng: 13.4, lat: 52.51 }
        }
      )
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
