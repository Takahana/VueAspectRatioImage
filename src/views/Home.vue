<template>
  <div class="home">
    <h1>256px x 256px</h1>
    <v-aspect-ratio-img
      src="background-sky.png"
      ref="image1"
      ref-name="image1"
      size="256x256"
    />

    <h1>20rem x 10rem</h1>
    <v-aspect-ratio-img
      src="background-sky.png"
      ref="image2"
      ref-name="image2"
      size="20x10"
      unit="rem"
    />

    <h1>width:100%で16:9</h1>
    <v-aspect-ratio-img
      src="background-sky.png"
      :fullWidth="true"
      ref="image3"
      ref-name="image3"
      :aspect-ratio="0.5625"
    />
  </div>
</template>

<script>
// @ is an alias to /src
import VueAspectRationImage from '@/components/VueAspectRatioImage.vue'

export default {
  name: "home",
  components: {
    'v-aspect-ratio-img': VueAspectRationImage
  }
};
</script>
