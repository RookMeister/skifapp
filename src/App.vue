<template>
  <div id="app">
    <yandex-map
      :coords="coords"
      :zoom="zoom"
      :controls="[]"
      @click="onClick"
    >
      <template v-for="(placemark, i) in placemarks">
        <ymap-marker
          v-if="placemark.visible"
           :key="i"
          :coords="placemark.coords"
          :marker-id="i"
          :hint-content="placemark.name"
        />
      </template>
    </yandex-map>
    <div class="marker" v-for="(placemark, i) in placemarks" :key="placemark.name">
      <div class="field">Маркер{{ i + 1 }}</div>
      <div class="field">
        <input @input="changeCoordinates(i)" v-model="placemark.coords[0]">
      </div>
      <div class="field">
        <input @input="changeCoordinates(i)" v-model="placemark.coords[1]">
      </div>
      <div class="field">
        <input v-model="placemark.name">
      </div>
      <div class="field">
        <input @input="toogleVisible(i)" type="checkbox" v-model="placemark.visible">
      </div>
      <div class="field"><button @click="deleteMarker(i)">Удалить</button></div>
    </div>
  </div>
</template>

<script>
import { yandexMap, ymapMarker, loadYmap } from 'vue-yandex-maps';

export default {
  name: 'App',
  components: {
    yandexMap,
    ymapMarker,
  },

  async mounted() {
    await loadYmap();
    this.map = window.ymaps;
  },

  data() {
    return {
      placemarks: [],
      map: null,
      zoom: 10,
      counter: 0,
      coords: [
        55.75399399999374,
        37.62209300000001,
      ],
    };
  },

  methods: {
    onClick(e) {
      this.counter += 1;
      const coords = e.get('coords');
      const marker = { coords, name: `Маркер${this.counter}`, visible: true };
      this.placemarks.push(marker);
    },
    changeCoordinates(i) {
      const [lat, lon] = this.placemarks[i].coords;
      this.placemarks[i].coords = [Number(lat), Number(lon)];
      this.coords = [Number(lat), Number(lon)];
    },
    deleteMarker(i) {
      this.placemarks.splice(i, 1);
    },
    toogleVisible(i) {
      console.log(this.map, i);
    },
  },

};
</script>

<style lang="less">
body {
  margin: 0;
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    width: 1000px;
    height: 500px;
    * {
      box-sizing: border-box;
    }
    .marker {
      padding: 8px;
      width: 100%;
      background-color: #2c3e50;
      color: #fff;
      display: flex;
      justify-content: space-between;
      // .field {

      // }
    }
  }
  .ymap-container {
    height: 100%;
    margin-bottom: 16px;
  }
}
</style>
