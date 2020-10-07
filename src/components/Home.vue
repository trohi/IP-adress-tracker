<template>
  <div class="container">
    <div class="layout">
      <div class="flex1">
        <h1>IP Address Tracker</h1>
      </div>
      <div class="flex2">
        <input type="text" placeholder="Search for any IP address or domain" v-bind="input">
        <img class="arrowImg" src="../assets/icon-arrow.svg" @click="getData">
      </div>
    </div>
    <div class="centralWrapper">
      <div>
        IP ADDRESS
        <p>{{this.input}}</p>
      </div>
      <v-divider vertical></v-divider>
      <div>
        LOCATION
        <p>{{this.city}} {{this.region}} {{this.postalCode}}</p>
        </div>
        <v-divider vertical></v-divider>
      <div>
        TIMEZONE
        <p>{{this.timeZone}}</p>
        </div>
        <v-divider vertical></v-divider>
      <div>
        ISP
        <p>{{this.isp}}</p>
        </div>
    </div>
    <div id="map">
      <l-map :zoom="LL.zoom" :center="LL.center">
        <l-tile-layer :url="LL.url" :attribution="LL.attribution"></l-tile-layer>
        <l-marker :lat-lng="LL.marker"> <img clas="marker" width="10" src="../assets/icon-location.svg" alt="marker"> </l-marker>
      </l-map>
    </div>
  </div>
</template>

<script>
  import L from 'leaflet';
  import { LMap, LTileLayer, LMarker } from 'vue2-leaflet';
  import { Icon } from 'leaflet';
  import fetch from 'node-fetch';

  export default {
    name: 'Home',
    data(){
      return {
        lat:'47.413220',
        lng:'-1.129482',
        input:'8.8.8.8',
        api_key:'at_jdwrNK6tuc3RR74pjYaKMY4wDotYM',
        city:'',
        region:'',
        postalCode:'',
        timeZone:'',
        isp:''
      }
    },
    components:{
      LMap,
      LTileLayer,
      LMarker
    },
    mounted(){
      delete Icon.Default.prototype._getIconUrl;
        Icon.Default.mergeOptions({
        iconRetinaUrl: require('leaflet/dist/images/marker-icon-2x.png'),
        iconUrl: require('../assets/icon-location.svg'),
        shadowUrl: require('leaflet/dist/images/marker-shadow.png'),
      });
    },
    computed:{
      LL(){
        return{
          zoom:13,
          center:L.latLng(this.lat, this.lng),
          url:'http://{s}.tile.osm.org/{z}/{x}/{y}.png',
          attribution:'&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
          marker:L.latLng(this.lat, this.lng)
        }
      }
    },
    methods:{
      getData(){
        fetch(`https://geo.ipify.org/api/v1?apiKey=${this.api_key}&ipAddress=${this.input}`)
        .then(res=>{
           return res.json()
        })
        .then(data=>{
          this.lat = data.location.lat;
          this.lng = data.location.lng
          this.city = data.location.city
          this.region = data.location.region
          this.postalCode = data.location.postalCode
          this.timeZone = data.location.timezone
          this.isp = data.location.isp
          console.log(data)
        })
      }
    }
  }
</script>

<style>
.container{
  margin:0px !important;
  padding:0px !important;
  width:100vw !important
}

.layout{
  display:flex;
  flex-direction: column;
  background-image: url('../assets/pattern-bg.png');
  background-size:cover;
  height:40vh;
  width:100vw;
  align-content: center
}

.layout div{
  margin-left:auto;
  margin-right:auto;
}

.flex1{
  margin-top:3%;
  margin-bottom:20px;
  color:white
}

.flex2{
  margin-bottom:auto;
  border-radius:15px !important;
  background-color:White !important;
  border-right: 50px solid rgb(49, 48, 48)
}

input{
  width:400px !important;
  padding-left:50px;
  height:50px;
  margin-left:auto;
  margin-right:auto;
}

.arrowImg{
  scale:1;
  margin-right:-30px;
  cursor:pointer
}

.centralWrapper{
  position: absolute;
  display:flex;
  z-index: 410;
  margin-left:17%;
  margin-right:35%;
  margin-top:-50px;
  height:18vh;
  width:70%;
  background-color:white;
  border-radius:10px;
}

.centralWrapper div{
  margin-left:auto;
  margin-right:auto;
  margin-top:3%;
  max-width:150px;
  font-size:12px;
  font-weight: bold;
  color:hsl(0, 0%, 63%)
}

.centralWrapper p{
  font-size:18px !important;
  color:black !important;
  margin-top:5px;
}

hr{
  margin-top:auto;
  margin-bottom:auto;
  height:70% !important;
  min-height: 50% !important;
  max-height: 70% !important
}

#map{
  height: 60vh;
  width:100vw;
}

.marker{
  scale:2
}

</style>
