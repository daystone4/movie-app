<template>
  <div class="loader">
      <span></span>
  </div>
</template>

<script>
export default {

}
</script>

<style lang="scss">
.loader {
  height: 100vh;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;

  @keyframes loading {
    to {
      transform: rotateZ(360deg);
    }
  }
  span {
    display: block;
    width: 5rem;
    height: 5rem;
    border-radius: 50%;
    border: 2px solid transparent;
    border-top-color: #fff;
    animation: loading 1200ms ease infinite;
  }
}
</style>