<template>
  <div class="level is-mobile">
    <div class="level-left">
      <img
        src="@/assets/LogoLong@0.1x.png"
        width="125px"
        alt="Lightweight UI components for Vue.js based on Bulma"
      />
    </div>
    <div class="level-right">
      <p>
        Hue Controller
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "Navbar"
};
</script>

<style lang="scss" scoped>
.level {
  padding: 0.5rem 1rem 0.5rem 1rem;
  background: rgb(3, 3, 27);
  border-top: 5px solid rgb(5, 197, 255);
  color: white;
  margin-bottom: 0.5rem !important;
}
</style>
