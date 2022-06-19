<template>
  <div class="item">
    <div v-if="setted">
      <div class="role">YOU ARE {{ role }}</div>
      <div class="info">You have to remain alive</div>
      <div class="time">{{ time }}</div>
      <div class="info">to get 100 points</div>
      <div class="map" :style="radarAngle">
        <div v-for="player in players">
          <div class="radar" :style="rotate(player.degree)">
            <div class="point"></div>
          </div>
        </div>
      </div>
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
      angle: 0,
      direction: 0,
      radarAngle: "rotate(0)deg",
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
    time() {
      return "3:48";
    },
    players() {
      return [
        {
          id: "1",
          degree: "0",
        },
        {
          id: "2",
          degree: "90",
        },
      ];
    }
  },

  created() {
    document.addEventListener.call(window, "deviceorientation", (event) => {
      this.radarAngle = `--radarAngle: rotate(${event.alpha}deg)`;
    });
  },

  mounted() {
    this.timer = setInterval(() => {
      this.getLocation();
    }, 10000);
  },

  beforeDestroy() {
    clearInterval(this.timer);
  },

  destroyed() {
    window.removeEventListener("deviceorientation", this.showDirection());
  },

  methods: {
    rotate(degree) {
      return {
        "--rotation": `rotate(${degree}deg)`,
      };
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
      );
    },
  },
};
</script>
<style scoped>
.radar {
  background-color: transparent;
  border-radius: 100%;
  height: 200px;
  width: 200px;
  position: absolute;
  transform: var(--rotation);
  transition: transform 0.7s linear;
  box-sizing: border-box;
  margin-top: 30px;
  margin-left: 22px;
}
.point {
  height: 10px;
  width: 10px;
  background-color: red;
  border-radius: 100%;
  position: absolute;
  top: -5px; /* -child size/2 */
  left: 95px; /* parent size/2 - child size/2 */
}
.map {
  height: 250px;
  width: 250px;
  position: relative;
  transform: var(--radarAngle);
  background-image: url("/images/radar.png");
  background-size: 100%;
  transition: transform 0.7s linear;
  margin-left: auto;
  margin-right: auto;
}
.time {
  font-size: 30px;
  border: 1px solid #00ff93;
  width: max-content;
  margin: 5px auto;
  padding: 5px 35px;
}
.info {
  font-size: 24px;
}
.role {
  font-size: 32px;
}
.time,
.info,
.role {
  text-align: center;
}
</style>
