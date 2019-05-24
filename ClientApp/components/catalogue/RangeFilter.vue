<template>

<!-- NOTE: MRD @callback="filter" was not working (not sure why), used @change instead -->
  <div class="slider">
    <vue-slider 
      @change="filter"
      :lazy=true
      :tooltip="'always'"
      :value="value"
      :formatter="formatter" 
      :min="min"
      :max="max"
      :interval="interval || 1"
      width="90%">
    </vue-slider>
  </div>
</template>

<script>
import vueSlider from "vue-slider-component";
// NOTE: MRD also add "vue-slider-component/theme/default.css" to vender array prop in webpack.config.vendor.js

export default {
  name: "range-filter",
  //data () { return {}}, // for testing
  components: {
    vueSlider
  },
  props: {
    min: {
      type: Number,
      required: true
    },
    max: {
      type: Number,
      required: true
    },
    interval: {
      type: Number
    },
    formatter: {
      type: String,
      required: true
    },
    minQueryKey: {
      type: String,
      required: true
    },
    maxQueryKey: {
      type: String,
      required: true
    }
  },
  computed: {
    value() {
      let rtn = [
        this.$route.query[this.minQueryKey] || this.min,
        this.$route.query[this.maxQueryKey] || this.max
      ]
      //console.log("init. val:" + rtn)   // testing
      return rtn;
    }
  },
  methods: {
    filter(values) {
      let query = Object.assign({}, this.$route.query);
      query[this.minQueryKey] = values[0];
      query[this.maxQueryKey] = values[1];
      this.$router.push({ query: query });
    }
  }
};
</script>

<style lang="scss" scoped>
.slider {
  padding: 35px 0 10px 10px;
}
</style>
