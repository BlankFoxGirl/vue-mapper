<template>
    <div class="vue-mapper-container" v-bind:style="styleObject">
      <div v-if="GetMode === 'default'" class="map-bind" :id="id"></div>
      <div class="vue-mapper-location-container" v-bind:style="styleObject" v-if="GetMode === 'Location'">
        <div class="inline-left">
          <div class="map-bind" :id="id"></div>
        </div>
        <div class="inline-right">
          Locations
        </div>
      </div>
    </div>
</template>

<script>
export default {
  name: 'VueMapper',
  props: {
    Center: {
      type: Object,
      default: null,
    },

    ApiKey: {
      type: String,
      default: '',
    },

    MapSize: {
      type: Object,
      default: null,
    },

    title: {
      type: String,
      default: 'Hej!',
    },

    mode: {
      type: String,
      default: '',
    },

    googleOverride: {
      type: Array,
      defaunt: null,
    },

    debug: {
      type: Boolean,
      default: false,
    },

    id_override: {
      type: String,
      default: null,
    },
  },
  data() {
    return {
      infoWindow: null,

      map: null,

      googleOptions: {
        center: {
          lat: -33.863276,
          lng: 151.107977,
        },

        zoom: 11,
        mapTypeId: 'roadmap',
      },

      thread: null,
    };
  },

  computed: {
    id() {
      let result = '';
      const characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
      const charactersLength = characters.length;

      for ( var i = 0; i < length; i++ ) {
          result += characters.charAt(Math.floor(Math.random() * charactersLength));
      }

      return (this.id_override !== null) ? this.id_override : result;
    },

    width() {
      return '100%';
    },

    height() {
      return '100px';
    },

    styleObject() {
      console.log('width: ' + this.width + '; height: ' + this.height);
      return {width: this.width + 'px', height: this.height + 'px'};
    },

    FocusLocation() {
      return (this.Center !== null) ? this.Center : { lat: -33.863276, lng: 151.107977 };
    },

    GetMode() {
      return (this.mode !== null) ? this.mode : 'default';
    },
  },
  mounted() {
    // Bring google maps into scope.
    this.importMap();
  },
  // Here we expect google to remain undefined.
  /* global google */
  /* eslint no-console: "off" */
  methods: {
    importMap () {
      if (this.debug) {
          console.log('Importing google maps with ' + this.ApiKey);
      }

      if (document.getElementById('GoogleAPIInclude') === null) {
        let dom = document.createElement('script');
        dom.id = 'GoogleAPIInclude';
        dom.src = 'https://maps.googleapis.com/maps/api/js?key=' + this.ApiKey;
        document.getElementsByTagName('body')[0].appendChild(dom);
      }

      this.thread = window.setInterval(() => {
        
          if (typeof google === 'object') {
              if (this.debug) {
                  console.log('Successfully lazy loaded google maps. Now we\'ll initiate the map!');
              }

              clearInterval(this.thread);

              if (this.debug) {
                  console.log(this.googleOptions);
              }

              if (this.FocusLocation !== null) {
                  this.googleOptions.center = this.FocusLocation;
              }

              this.map = new google.maps.Map(document.getElementById(this.id), this.googleOptions);
              this.infoWindow = new google.maps.InfoWindow();
          }
      }, 100);
    },
  },
};
</script>

<style>
div#app {
  height: 80vh;
}

div.vue-mapper-container {
  height: 100%;
  width: 100%;
}

div.vue-mapper-location-container {
  width: 100%;
  min-width: 300px;
  height: 100%;
}

div.vue-mapper-location-container > div.inline-left {
  display: inline-flex;
  width: 70%;
  height: 100%;
}

div.vue-mapper-location-container > div.inline-right {
  display: inline-flex;
  width: 30%;
  height: 100%;
  vertical-align: top;
}

div.map-bind {
  height: 100%;
  width: 100%;
}
</style>