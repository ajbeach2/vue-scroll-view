<template>
  <div class="thumbnail2" v-bind:style="{transform: transform}">
    {{id}}
    <img style="height:100%" :src="uri" alt="...">
  </div>
</template>

<script>
export default {
  data () {
    return {
    }
  },
  props: [
    'id',
    'title',
    'uri',
    'caption',
    'height',
    'width',
    'transform'
  ]
}
</script>
<style type="text/css">
  .thumbnail2 {
    position: absolute;
    border-color: black;
    height: 200px !important;
    margin:0px;
    border: 2px solid black;
  }
</style>
