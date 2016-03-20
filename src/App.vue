<template>
  <div id="content">
    <div style="position: fixed">lower {{lowerLimit}}, higher {{higherLimit}}, diff {{higherLimit - lowerLimit}}</div>
    <hello v-for="elm in vertical"
      :caption="elm.image.caption"
      :title="elm.image.title"
      :uri="elm.image.display_sizes[0].uri"
      :transform="elm.transform"
      :track-by="$index"
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
      vertical: []
    }
  },
  ready () {
    window.onscroll = (e) => {
      this.scrollTop = window.scrollY
    }
  },

  computed: {
    vertical () {
      var rowsAbove = Math.floor(this.scrollTop / this.elementHeight)
      var visibleRows = Math.ceil(((rowsAbove * this.elementHeight) + this.windowHeight) / this.elementHeight)

      var extra = Math.ceil(visibleRows / 2)
      var lowerLimit = (rowsAbove)
      var higherLimit = (visibleRows + extra * 2)

      this.lowerLimit = lowerLimit
      this.higherLimit = lowerLimit + 10

      var elementsToRender = []
      for (var index = lowerLimit; index < higherLimit; index++) {
        elementsToRender.push({
          id: this.images[index].id,
          transform: 'translate(0px,' + (index * this.elementHeight) + 'px)',
          height: this.elementHeight + 'px',
          image: this.images[index]
        })
      }
      // wtf this keeps getting lager
      console.log(elementsToRender.length)
      return elementsToRender
    }
  },

  components: {
    Hello
  }
}
</script>
