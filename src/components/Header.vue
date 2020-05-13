<template>
  <div>
  <b-nav tabs>
    <b-nav-item disabled><b>Hauskoja kyssäreitä</b></b-nav-item>
    <b-nav-item disabled>Laskuri: {{xNumOikein}}/{{xNumTotal}} </b-nav-item>
  </b-nav>
</div>
</template>

<script>
export default {
  props:[
    'xNumOikein',
    'xNumTotal'
  ]
}
</script>