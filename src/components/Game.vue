<template>
  <div class="item">
    <div v-if="setted">
      <div class="role">YOU ARE {{ role }}</div>
      <div class="info">You have to remain alive</div>
      <div class="time">{{ timeRow }}</div>
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
import { v4 as uuidv4 } from 'uuid';

export default {
  data() {
    return {
      location: null,
      gettingLocation: false,
      latitude: null,
      longitude: null,
      errorStr: null,
      timer: null,
      oldAngle: 0,
      currentAngle: 0,
      direction: 0,
      radarAngle: "rotate(0)deg",
      gameTime: 10 * 60,
      players: [],
      role: "",
    };
  },
  
  computed: {
    setted() {
      return true;
    },
    loaded() {
      return true;
    },
    timeRow() {
      let minutes = Math.floor(this.gameTime / 60);
      if (minutes < 10) {
        minutes = "0" + minutes;
      }
      let seconds = this.gameTime % 60;
      if (seconds < 10) {
        seconds = "0" + seconds;
      }

      return `${minutes}:${seconds}`;
    },
  },

  created() {
    document.addEventListener.call(window, "deviceorientation", (event) => {
      const a = this.oldAngle;
      const b = event.alpha;
      const rawDiff = b - a;
      const finalDiff = (Math.abs(rawDiff) > 180)
        ? (a > b
              ? 360 - a + b
              : b - a - 360)
        : rawDiff;

      this.oldAngle = this.currentAngle;
      this.currentAngle += finalDiff;
      this.radarAngle = `--radarAngle: rotate(${this.currentAngle}deg)`;
    });
  },

  mounted() {
    this.timer = setInterval(() => {
      this.getLocation();

      // This will send outdated data, take in consideration
      ws.send(JSON.stringify({
        event: "telemetry",
        data: {
          longitude: this.longitude,
          latitude: this.latitude,
        },
      }));
    }, 200);

    this.countdownIntervalToken = setInterval(() => {
      this.gameTime--;
      if (this.gameTime === 0) {
        clearInterval(this.countdownIntervalToken);
      }
    }, 1000);

    const ws = new WebSocket("ws://67.207.80.245:8000/ws/test/" + uuidv4());
    this.ws = ws;
    ws.onmessage = ({ data: payload }) => {
      const { event, data } = JSON.parse(payload);

      switch (event) {
        case "enemies":
          this.players = data.map(d => ({ degree: d }));
          break;

        case "gameStatusUpdate":
          if (data.status === "start") {
              console.log("STARTED", data);
              this.role = data.role.toUpperCase();
          }
          else if (data.status === "finish") {
            clearInterval(this.countdownIntervalToken);
          }
          break;

        default:
          console.error("Unrecognized server message", event, data);
      }
    };

    ws.onopen = () => {
      ws.send(JSON.stringify({ event: "startGame", data: {} }));
    }
  },

  beforeUnmounted() {
    clearInterval(this.timer);
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
