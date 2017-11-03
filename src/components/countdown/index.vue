<template>
  <span>{{countdown}}</span>
</template>

<script>
export default {
  name: 'countdown',
  props: {
    value: Number,
    start: {
      type: Boolean,
      default: true
    },
    format: {
      type: String,
      default: 'ss'
    }
  },
  created () {
    this.currentTime = this.time
    if (this.value) this.currentTime = this.value
  },
  methods: {
    tick () {
      let _this = this
      this.interval = setInterval(function () {
        if (_this.currentTime > 0) {
          _this.currentTime--
          if (_this.format === 'ss') {
            _this.countdown = _this.currentTime
          } else if (_this.format === 'mm:ss') {
            let mm = Math.floor(_this.currentTime / 60)
            let ss = _this.currentTime - mm * 60
            mm = mm < 10 ? '0' + mm : mm
            _this.countdown = mm + ':' + ss
          } else if (_this.format === 'hh:mm:ss') {
            let hh = Math.floor(_this.currentTime / 3600)
            let mm = Math.floor((_this.currentTime - hh * 3600) / 60)
            let ss = _this.currentTime - mm * 60
            hh = hh < 10 ? '0' + hh : hh
            mm = mm < 10 ? '0' + mm : mm
            _this.countdown = hh + ':' + mm + ':' + ss
          }
        } else {
          _this.stop()
          _this.index++
          _this.$emit('on-finish', _this.index)
        }
      }, 1000)
    },
    stop () {
      clearInterval(this.interval)
    }
  },
  watch: {
    value (val) {
      this.currentTime = val
    },
    currentTime (val) {
      this.$emit('input', val)
    },
    start (newVal, oldVal) {
      if (newVal === true && oldVal === false && this.currentTime > 0) {
        this.tick()
      }
      if (newVal === false && oldVal === true) {
        this.stop()
      }
    }
  },
  mounted () {
    if (this.start) {
      this.tick()
    }
  },
  data () {
    return {
      interval: null,
      index: 0,
      currentTime: 60,
      countdown: ''
    }
  }
}
</script>
