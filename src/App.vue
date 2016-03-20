<template>
  <div id="content" v-bind:style="{height: height + 'px'}">
    <div style="position: fixed; z-index: 99">lower {{lowerLimit}}, higher {{higherLimit}}, diff {{higherLimit - lowerLimit}} height {{height}}</div>
    <hello v-for="elm in vertical"
      :caption="elm.image.caption"
      :title="elm.image.title"
      :uri="elm.image.display_sizes[0].uri"
      :transform="elm.transform"
      :trackBy="elm.id"
    ></hello>
  </div>
</template>

<script>
import Hello from './components/Hello'
import data from './assets/mock.json'

export default {
  data () {
    return {
      pages: [],
      images: data.images,
      loaded: false,
      scrollDelta: 0,
      scrollTop: 0,
      elementHeight: 200,
      windowWidth: global.innerWidth,
      windowHeight: global.innerHeight,
      lowerLimit: 0,
      higherLimit: 0,
      visibleRows: 0
    }
  },
  ready () {
    window.onscroll = (e) => {
      this.scrollTop = window.scrollY
    }
  },

  computed: {
    height () {
      return this.images.length * this.elementHeight
    },
    vertical () {
      var rowsAbove = Math.floor(this.scrollTop / this.elementHeight)
      var visibleRows = Math.ceil(((rowsAbove * this.elementHeight) + this.windowHeight) / this.elementHeight)

     // var extra = Math.ceil(visibleRows / 2)
      var lowerLimit = rowsAbove
      var higherLimit = 2 + rowsAbove + Math.floor(this.windowHeight / this.elementHeight)

      this.lowerLimit = lowerLimit
      this.higherLimit = higherLimit
      this.visibleRows = visibleRows

      var elementsToRender = []
      for (var index = lowerLimit; index < higherLimit; index++) {
        if (index >= this.images.length) return elementsToRender
        elementsToRender.push({
          id: this.images[index]['id'],
          transform: 'translate(0px,' + (index * this.elementHeight) + 'px)',
          height: this.elementHeight + 'px',
          image: this.images[index]
        })
      }
      console.log(elementsToRender.length)
      return elementsToRender
    }
  },

  components: {
    Hello
  }
}
</script>
<style type="text/css">
  #content {
    position: relative
  }
</style>
