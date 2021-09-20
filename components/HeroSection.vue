<template>
    <section class="hero">
        <!-- <img src="../assets/images/movie-popcorn.jpeg" alt=""> -->
        <div class="hero__content">
            <!-- <div class="hero__content-text"> -->
                <h3 class="small-heading">Now Streaming...</h3>
                <h1 class="big-heading">Now Streaming</h1>
                <a href="#movies-grid" class="btn">View Movies</a>
            <!-- </div> -->
        </div>
    </section>
</template>

<script>
export default {

}
</script>

<style lang="scss" scoped>
.hero {
    position: relative;
    min-height: 80vh;
    min-width: 100%;
    position: relative;
    background-image: url('~assets/images/movie-popcorn.jpeg');
    background-repeat: no-repeat;
    background-size: cover;

  &::after {
    content: '';
    position: absolute;
    inset: 0;
    height: 100%;
    width: 100%;
    background-color: rgba($dark, 0.5);
  }

  &__content {
    z-index: 1;
    position: absolute;
    inset: 0;
    padding: 2rem;

    & > * {
        margin-bottom: 0.5rem;
    }

    .small-heading {
      font-weight: 700;
      font-size: clamp(1rem, 2vw, 1.5rem);
      font-style: italic;
      text-transform: uppercase;
      color: $brown;
    }
  
    .big-heading {
      color: $white;
      font-size: clamp(4rem, 4vw + 1rem, 5rem);
      font-weight: 300;

    }
    .btn {
      font-size: 1.5rem;
    }
  }
}
</style>