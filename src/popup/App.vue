<template>
  <section class="app">
    <Navbar />
    <div class="container has-text-centered">
      <h3 class="heading">Toggle Lights</h3>
      <center>
        <div class="block">
          <p
            v-for="(light, index) in lightState"
            :key="index"
            @click="toggleLight(light.id)"
            class="button is-light is-outlined"
            :class="{ 'is-success': light.state.on }"
          >
            {{ light.name }}
          </p>
        </div>
      </center>
    </div>
  </section>
</template>

<script>
import Navbar from "@/components/Navbar.vue";

export default {
  name: "App",
  components: { Navbar },
  data() {
    return {
      hueEndpoint:
        "https://10.0.0.223:443/api/V9g-OYemWVq5tQta02BmYJpDfN0FgXqWmmGZlWcL",
      lightState: undefined,
      rawLightState: undefined,
      errors: undefined
    };
  },
  mounted() {
    this.getLightState();
  },
  methods: {
    getLightState() {
      const hueScope = "/lights/";
      const builtURL = this.hueEndpoint + hueScope;
      this.axios
        .get(builtURL)
        .then(response => {
          this.rawLightState = response;
          let lights = [];
          Object.entries(response.data).forEach(([key, val]) => {
            let light = val;
            light.id = key;
            lights.push(val);
          });
          console.log(lights);
          this.lightState = lights;
        })
        .catch(err => {
          console.log(err);
        });
    },
    toggleLight(id) {
      const hueScope = "/lights/";
      const builtURL = this.hueEndpoint + hueScope + id + "/state";
      console.log(this.rawLightState);
      const curLightState = this.rawLightState.data[id].state.on;
      const body = { on: !curLightState };
      this.axios
        .put(builtURL, body)
        .then(res => {
          console.log(res);
          this.getLightState();
        })
        .catch(err => {
          console.log(err);
          this.errors = err;
        });
    }
  }
};
</script>

<style lang="scss">
html {
  background: #000031 !important;
  color: ghostwhite;
  border-radius: 20px;
  width: 350px;
}

.container {
  padding: 0rem 1rem 1rem 1rem;
  h2.subtitle {
    color: ghostwhite;
  }
}
.heading {
  color: ghostwhite;
  margin-bottom: 1rem !important;
}
.button {
  border-radius: 0 !important;
  font-weight: bold;
  margin-left: 0.25rem;
  margin-right: 0.25rem;
}

@import "~bulma/sass/utilities/_all";

// Set your colors
$primary: #8c67ef;
$primary-invert: findColorInvert($primary);
$twitter: #4099ff;
$twitter-invert: findColorInvert($twitter);

// Setup $colors to use as bulma classes (e.g. 'is-twitter')
$colors: (
  "white": (
    $white,
    $black
  ),
  "black": (
    $black,
    $white
  ),
  "light": (
    $light,
    $light-invert
  ),
  "dark": (
    $dark,
    $dark-invert
  ),
  "primary": (
    $primary,
    $primary-invert
  ),
  "info": (
    $info,
    $info-invert
  ),
  "success": (
    $success,
    $success-invert
  ),
  "warning": (
    $warning,
    $warning-invert
  ),
  "danger": (
    $danger,
    $danger-invert
  ),
  "twitter": (
    $twitter,
    $twitter-invert
  )
);

// Links
$link: $primary;
$link-invert: $primary-invert;
$link-focus-border: $primary;

// Import Bulma and Buefy styles
@import "~bulma";
@import "~buefy/src/scss/buefy";
</style>
