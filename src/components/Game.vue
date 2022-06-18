<template>
  <div class="item">
    <div v-if="setted">
      <div>Your role is: {{ role }}</div>
      <div>Find the prey players</div>
      <div class="map"></div>
      <div>Longitude: {{ longitude }}, latitude: {{ latitude }}</div>
      <div><router-link to="/main">Leave</router-link></div>
    </div>
    <div v-else>
      <div v-if="loaded">
        <div>Your role is: {{ role }}</div>
        <div>
          <p>
            Please, follow the indicated direction, this is your starting point
          </p>
        </div>
        <div class="map"></div>
        <div>
          <router-link to="/main">Leave</router-link>
        </div>
      </div>
      <div v-else>
        <p>Setting you up...</p>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      location: null,
      gettingLocation: false,
      latitude: null,
      longitude: null,
      errorStr: null,
      timer: null,
    };
  },
  computed: {
    role() {
      return "Hunter";
    },
    setted() {
      return true;
    },
    loaded() {
      return true;
    },
  },
  mounted() {
    this.$window.addEventListener('deviceorientation', this.showDirection(event));
    this.timer = setInterval(() => {
      this.getLocation()
    }, 10000)
  },
  beforeDestroy() {
    clearInterval(this.timer)
  },
  destroyed() {
    this.window.removeEventListener('deviceorientation', this.showDirection());
  },
  methods: {
    fetchInitialPosition() {
      axios
        .get("https://api.coindesk.com/v1/bpi/currentprice.json")
        .then((response) => (this.info = response));
    },
    fetchStart() {
      axios
        .get("https://api.coindesk.com/v1/bpi/currentprice.json")
        .then((response) => (this.info = response));
    },
    showDirection(event) {
      console.log(event.alpha)
    },
    getLocation() {
      navigator.geolocation.getCurrentPosition(
        (pos) => {
          this.gettingLocation = false;
          this.location = pos;
          this.latitude = this.location.coords.latitude;
          this.longitude = this.location.coords.longitude;
          console.log(this.location);
        },
        (err) => {
          this.gettingLocation = false;
          this.errorStr = err.message;
        }
      )
    },
  },
}
</script>
