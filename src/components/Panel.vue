<template>
  <div class="box">
    <nav class="navheader">
      <div class="menuH">
        <span class="menuopen">
          <img src="static/menu.svg" width="25" height="25" alt />
        </span>
        <span class="menuclose">
          <img src="static/close.svg" width="25" height="25" alt />
        </span>
      </div>
      <a class="nav--logo" href>
        <img src="static/logo.png" width height="40" alt />
      </a>
    </nav>
    <div class="box--flex">
      <aside class="aside--bar">
        <h2>Filtros</h2>
        <div class="box-itm">
          <h3>Clasificación</h3>
          <select
            name
            v-model="filtros.classification"
            class="form-control"
            id
            @change="addFilter($event)"
          >
            <option>Todos</option>
            <option v-for="(c,index) in classifications" :value="c" v-bind:key="index">{{c}}</option>
          </select>
        </div>
        <div class="box-itm">
          <h3>Grupo</h3>
          <select name v-model="filtros.group" class="form-control" id @change="addFilter($event)">
            <option>Todos</option>
            <option v-for="(g,index) in groups" :value="g" v-bind:key="index">{{g}}</option>
          </select>
        </div>
      </aside>
      <div class="map">
        <here-map :classification="filtros.classification" :group="filtros.group" />
      </div>
    </div>
  </div>
</template>
<script>
import axios from 'axios'
import HereMap from './HereMap'
import records from '@/data/records'

const lines = records.split('\n')
const points = lines.map(record => {
  const [, , , , latitud, longitud, , , , tipo, classification, size, , , , , grupo] = record.split(',')
  return { latitud, longitud, tipo, classification, size, grupo }
})
const headers = points.splice(0, 1)

export default {
  name: 'Panel',
  components: {
    HereMap
  },
  data() {
    return {
      mapa: null,
      classifications: [...new Set(points.map(p => p.classification))],
      groups: [...new Set(points.map(p => p.grupo))],
      filtros: {
        classification: 'Todos',
        group: 'Todos'
      }
    }
  },
  mounted() {
    // this.getdatos()
    // SCRIPT PARA ABRIR EL MENU RESPONSI Y CERRAR
    $('.menuH .menuopen').click(function(event) {
      $(this).addClass('active')
      $('.aside--bar').addClass('active')
      $('.map').addClass('active')
      $('.menuclose').addClass('active')
    })
    $('.menuH .menuclose').click(function(event) {
      $(this).removeClass('active')
      $('.menuopen').removeClass('active')
      $('.map').removeClass('active')
      $('.aside--bar').removeClass('active')
    })

    // END MENU
  },
  methods: {
    getdatos() {
      console.log('datos')
    },

    addFilter(event) {
      $('.preload').css('display', 'block')
      var rows = []
      console.log('Filtros', this.filtros)
    },

    createMap() {
      return null
    },

    createMarkers(locations, map) {}
  }
}
$(document).ready(function() {
  $('.box-itm select').change(function(event) {
    if ($(window).width() <= 768) {
      $('.menuH .menuclose').click()
    }
  })
})
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style >
.tooltip_1.active .tooltip {
  opacity: 1 !important;
  margin-top: -20px !important;
}
.tooltip.top.in {
  opacity: 0 !important;
}
.slider.slider-horizontal {
  margin-left: 15px;
  width: 260px !important;
}
.btn_aside {
  margin-top: 30px;
  text-align: center;
}
.btn_aside img {
  width: 15px;
  height: 15px;
}
.boxinput {
  width: 48%;
  display: inline-block;
  vertical-align: top;
  font-size: 15px;
  padding: 3px 10px;
  line-height: 100%;
}

.boxinput input[type='checkbox'] {
  position: absolute;
  top: 0px;
  left: 0;
}
.boxinput span {
  display: inline-block;
  margin-left: 15px;
}
.boxinput label {
  position: relative;
}
.box {
  overflow: hidden;
}
.menuH {
  position: absolute;
  top: 0;
  left: 20px;
  bottom: 0;
  width: 40px;
  height: 40px;
  line-height: 38px;
  margin: auto;
  opacity: 0;
  visibility: hidden;
  background: #30a9a0;
  padding: 10px;
  border-radius: 6px;
}
.menuH span {
  display: block;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  margin: auto;
  cursor: pointer;
  width: 24px;
}
.menuclose {
  opacity: 0;
  visibility: hidden;
}
.menuclose.active {
  opacity: 1;
  visibility: visible;
}
.menuopen.active {
  opacity: 0;
  visibility: hidden;
}
.navheader {
  padding: 10px 0;
  position: relative;
}
.box--filter {
  background: #e8e8e8;
  padding: 15px 10px;
  border-bottom: 1px solid #d9d9d9;
  position: relative;
  font-weight: 500;
  color: #797979;
}
.box--filter i {
  width: 0;
  height: 0;
  border-style: solid;
  border-width: 0 4px 7px 4px;
  border-color: transparent transparent #a7a7a7 transparent;
  position: absolute;
  top: 0;
  bottom: 0;
  margin: auto;
  right: 10px;
}
.aside--bar {
  width: 25%;
  height: calc(100vh - 60px);
  background: #f3f3f3;
  padding: 20px;
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
  text-align: left;
  overflow: auto;
}
.map {
  width: 75%;
  height: calc(100vh - 60px);
  position: relative;
}
.map.active {
  right: -300px;
}
.box--price {
  display: block;
}
.box--flex {
  display: -webkit-flex;
  display: -moz-flex;
  display: -ms-flex;
  display: -o-flex;
  display: flex;
  justify-content: center;
  position: relative;
}
.box-itm {
  padding: 30px 0;
  border-bottom: 1px solid #e2e2e2;
}
.box-itm h3 {
  font-size: 16px;
  font-weight: bold;
  color: #797979;
  font-family: 'arial', sanserif;
}
.tooltip.top {
  opacity: 1 !important;
  display: block !important;
  margin-top: 22px !important;
}
@media screen and (max-width: 1024px) {
  .aside--bar {
    width: 40%;
  }
  .map {
    width: 60%;
  }
}
@media screen and (max-width: 768px) {
  .menuH {
    opacity: 1;
    visibility: visible;
  }
  .aside--bar {
    left: -300px;
    position: absolute;
    top: 0;
    width: 300px;
  }
  .aside--bar.active {
    left: 0;
  }
  .map {
    width: 100%;
  }
}
</style>
