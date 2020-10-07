<template>
  <div class="container">
    <div class="layout">
      <div class="flex1">
        <h1>IP Address Tracker</h1>
      </div>
      <div class="flex2">
        <input type="text" placeholder="Search for any IP address or domain"  :value="input">
        <img class="arrowImg" src="../assets/icon-arrow.svg" @click="getData">
      </div>
    </div>
    <div class="centralWrapper">
      <div>
        IP ADDRESS
        <p>{{this.input}}</p>
      </div>
      <v-divider vertical class="hidden-sm-and-down"></v-divider>
      <v-divider class="hidden-sm-and-up"></v-divider>
      <div>
        LOCATION
        <p>{{this.city}} {{this.region}} {{this.postalCode}}</p>
        </div>
        <v-divider vertical class="hidden-sm-and-down"></v-divider>
        <v-divider class="hidden-sm-and-up"></v-divider>
      <div>
        TIMEZONE
        <p>{{this.timeZone}}</p>
        </div>
        <v-divider vertical class="hidden-sm-and-down"></v-divider>
        <v-divider class="hidden-sm-and-up"></v-divider>
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
        lat:'34.0614',
        lng:'-118.08162',
        input:'192.212.174.101',
        api_key:'at_jdwrNK6tuc3RR74pjYaKMY4wDotYM',
        city:'Rosemead (Garvey)',
        region:'California',
        postalCode:'91770',
        timeZone:'UTC-07:00',
        isp:'SpaceX Starlink'
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
  width:400px;
  margin-bottom:auto;
  border-radius:15px !important;
  background-color:White !important;
  border-right: 50px solid rgb(49, 48, 48)
}

input{
  width:100%;
  padding-left:50px;
  height:50px;
  margin-left:auto;
  margin-right:auto;
}

.arrowImg{
  scale:1;
  margin-right:-8%;
  margin-left:4%;
  cursor:pointer
}

.centralWrapper{
  position: absolute;
  display:flex;
  z-index: 410;
  margin-left:17%;
  margin-right:35%;
  margin-top:-50px;
  min-height:18vh;
  max-height: 100vh;
  width:70%;
  background-color:white;
  border-radius:10px;
}

.centralWrapper div{
  margin-left:auto;
  margin-right:auto;
  margin-top:3%;
  max-width:150px;
  font-size:14px;
  letter-spacing: 2px;
  font-weight: bold;
  color:hsl(0, 0%, 63%)
}

.centralWrapper p{
  font-size:18px !important;
  color:black !important;
  margin-top:5px;
  letter-spacing: initial
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

@media only screen and (max-width: 385px){
  .flex2{
    width:90% !important
  }

  #map{
    height: 64vh;
    width:auto;
}

  input{
    margin-left:-40px;
    width: 110%
  }

  .arrowImg{
    position:absolute;
    margin-top:5%;
    margin-right:0px;
    margin-left:8%
  }

  .centralWrapper{
    flex-direction: column;
    margin-top:-30%;
    height:auto
  }

  .centralWrapper div{
    height:auto;
    text-align: center;
    max-width:200px
  }
}

</style>
