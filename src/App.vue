<template>
  <div id="app">
    <img id="background" src="./assets/background.jpg" />

    <img id="sun" src="./assets/sun.png" :style="sunStyle" />
    <div :style="rocketStyle" id="rocket">
      <img src="./assets/rocket-icon.png" style="transform: rotate(65deg);" />
    </div>
  </div>
</template>

<script>
function toRadians(degrees) {
  const pi = Math.PI;
  return degrees * (pi / 180);
}

export default {
  name: "app",
  components: {},
  data() {
    return {
      sunPosition: { x: 1300, y: 300 },
      steg: 25,
      vinkel: 0,
      position: { x: 0, y: 0 },
      traveling: false
    };
  },
  mounted() {
    window.addEventListener("keydown", this.onKeyPress);

    setInterval(() => {
      if (this.traveling === false) return;

      const angule = toRadians((360 - this.vinkel) % 360);
      const cos = Math.cos(angule);
      const sin = Math.sin(angule);

      this.position = {
        x: this.position.x + this.steg * cos,
        y: this.position.y + this.steg * sin
      };
    }, 50);
  },
  computed: {
    rocketStyle() {
      return {
        transform: `rotate(${this.vinkel}deg)`,
        left: this.position.x + "px",
        bottom: this.position.y + "px"
      };
    },
    sunStyle() {
      return {
        left: this.sunPosition.x + "px",
        bottom: this.sunPosition.y + "px"
      };
    },
    isNearSun() {
      const xDiff = this.position.x > this.sunPosition.x;
      const yDiff = this.position.y > this.sunPosition.y;
      return xDiff && yDiff;
    }
  },
  watch: {
    isNearSun() {
      // alert(':)');
    }
  },
  methods: {
    onKeyPress(e) {
      if (e.key === "ArrowRight") {
        this.vinkel = this.vinkel + this.steg;
      }
      if (e.key === "ArrowLeft") {
        this.vinkel = this.vinkel - this.steg;
      }
      if (e.which === 32) {
        this.traveling = !this.traveling;
      }

      if (e.key === "r") {
        this.vinkel = 0;
        this.traveling = false;
        this.position = { x: 0, y: 0 };
      }

      e.preventDefault();
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#background {
  position: absolute;
  width: 100vw;
  left: 0;
  top: 0;
}
#rocket {
  transition: transform 0.3s, bottom 0.3s linear, left 0.3s linear;
  position: absolute;
}
#rocket img {
  width: 125px;
}
#sun {
  position: absolute;
  width: 400px;
}
</style>
