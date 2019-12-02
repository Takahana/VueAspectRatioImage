<template>
<div class="v-aspect-ratio-img" :style="style">
  <img :src="imageData">
</div>
</template>
<script lang="ts">
import { Vue, Component, Prop } from 'vue-property-decorator'
@Component
export default class VueAspectRatioImage extends Vue {
  @Prop({ type: String, required: false, default: '128x128' })
  size!: string

  @Prop({ type: Number, required: false, default: 0 })
  aspectRatio!: number // height/width

  @Prop({ type: Boolean, required: false, default: false })
  fullWidth!: boolean

  @Prop({ type: String, required: false, default: null })
  refName!: string|null

  @Prop({ type: String, required: false, default: 'px' })
  unit!: string

  @Prop({ type: String, default: '', required: false })
  src!: string

  clientWidth: number|null = null

  mounted() {
    this.handleResize()
    // 画面サイズ変更イベント
    window.addEventListener('resize', this.handleResize)
  }
  beforeDestory() {
    window.removeEventListener('resize', this.handleResize)
  }

  handleResize() {
    if (this.refName) {
      const vc = this.$parent.$refs[this.refName] as any
      if (vc && Array.isArray(vc)) {
        try {
          vc[0].$el.style.minWidth = '0'
          this.clientWidth = vc[0].$el.clientWidth
        } catch {
          this.clientWidth = null
        }
      } else if (vc) {
        vc.$el.style.minWidth = '0'
        this.clientWidth = vc.$el.clientWidth
      }
    }
  }

  get imageData() {
    if (this.src.indexOf('http') === 0 || this.src.indexOf('data:image') === 0) {
      return this.src
    } else if (this.src) {
      return require('../assets/' + this.src)
    } else {
      return ''
    }
  }

  get style() {
    let width = '128'
    let height = '128'
    if (/\d+x\d+/.test(this.size)) {
      width = this.size.split('x')[0]
      height = this.size.split('x')[1]
    }

    if (this.fullWidth && this.refName && this.clientWidth) {
      width = String(this.clientWidth)
    }

    if (this.aspectRatio > 0) {
      height = String(this.aspectRatio * parseInt(width))
    }

    return {
      width: this.fullWidth ? '100%' : `${width}${this.unit}`,
      height: `${height}${this.unit}`,
      minWidth: `${width}${this.unit}`
    }
  }
}
</script>
<style lang="stylus" scoped>
.v-aspect-ratio-img
  img
    width 100%
    height 100%
    object-fit cover
</style>
