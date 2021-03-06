<template>
  <div id="app" class="scroll-snap">
    <section class="section header scroll-snap-center">
      <h1>
        {{ name }}
        <small>v{{ version }}</small>
      </h1>
      <p class="lead">{{ description }}</p>
      <p v-if="loaded" class="instruction">The logo is loaded! 👍</p>
      <p v-else-if="loading" class="instruction">The logo is currently loading…</p>
      <p v-else class="instruction">
        Scroll down until the logo appears…
        <br />⬇︎
      </p>
    </section>
    <section class="section demo scroll-snap-center">
      <ImageLazy src="/images/animated-logos.gif" class="logo" @loading="loading = true" @load="loaded = true" />
    </section>
    <section class="section photos scroll-snap-center">
      <ImageLazy v-for="(photo, index) in photos" :key="photo" :src="photo" :delay="index * 100" class="photo" />
    </section>
  </div>
</template>

<script>
import ImageLazy from '@/components/ImageLazy.vue'

export default {
  name: 'app',
  components: {
    ImageLazy
  },
  data () {
    const { name, version, description } = this.$root.$options.packageInfo
    return {
      name,
      version,
      description,
      loading: false,
      loaded: false,
      photos: []
    }
  },
  created () {
    for (let i = 0 ; i < 50; i++) {
      this.photos.push(`https://source.unsplash.com/random/200x200`)
    }
  }
}
</script>

<style>
:root {
  --brand-primary: #2c3e50;
  --bg-color: #2c3e5033;
  --bg-color-alt: #141c24;
}
html,
body {
  margin: 0;
  padding: 0;
  height: 100%;
  width: 100%;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  -ms-overflow-style: -ms-autohiding-scrollbar;
  font-family: "Helvetica Neue", "Ubuntu", "Roboto", "Noto", "Segoe UI", "Arial",
    sans-serif;
  color: var(--brand-primary);
}
*,
*::before,
*::after {
  box-sizing: border-box;
}
#app {
  text-align: center;
  height: 100vh;
  overflow: auto;
}
.scroll-snap {
  scroll-snap-type: y mandatory;
}
.scroll-snap-center {
  /* scroll-snap-stop: always; */
  scroll-snap-align: center;
}
.section {
  height: 100vh;
}
.header {
  padding: 60px 0;
}
.header h1 {
  font-size: 2em;
}
.header h1 small {
  display: block;
  font-size: 60%;
  font-weight: normal;
}
.lead {
  font-size: 120%;
}
.instruction {
  margin-top: 20vh;
}
.demo {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: var(--bg-color);
}
.logo {
  width: 100%;
  max-width: 600px;
}
.photos {
  padding: 2em 0;
  overflow: hidden;
  display: grid;
  grid-template-columns: repeat(auto-fill, 100px);
  grid-auto-rows: 100px;
  grid-gap: 2px;
  justify-content: center;
  align-items: center;
  background-color: var(--bg-color-alt);
}
.photo {
  width: 100px;
  height: 100px;
  object-fit: cover;
}

.image-lazy {
  opacity: 0;
}
.image-lazy-loading.photo {
  transform: translateX(100vh);
}
.image-lazy-loading.logo {
  transform: rotate(360deg);
}
.image-lazy-loaded {
  transition: all 1s ease;
  opacity: 1;
}

</style>
