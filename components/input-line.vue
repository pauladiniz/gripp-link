<template>
  <div class="line">
    <span class="f-pink">{{ user }}</span>
    <span class="f-foreground">@</span>
    <span class="f-yellow">gripp.link</span>
    <span class="f-foreground">:</span>
    <div class="input">
      <span class="f-foreground">$</span>
      <input
        v-model="input"
        @keyup.enter="onEnter"
        @blur="blur"
      >
      <i />
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import { mapGetters, mapMutations } from 'vuex'

export default Vue.extend({
  name: 'InputLIne',

  props: {
    user: {
      type: String,
      required: true
    },
    blur: {
      type: Function,
      required: true
    }
  },

  data: () => ({
    input: '',
    knowCommands: [
      'clear',
      'history',
      'dorme dorme',
      'about',
      'links',
      'help',
      'carrer'
    ]
  }),

  computed: {
    ...mapGetters([
      'getInput'
    ])
  },
  mounted () {
    (this.$el.querySelector('.input input') as HTMLElement).focus()
    sessionStorage.setItem('history', JSON.stringify(['']))
  },
  methods: {
    ...mapMutations([
      'setInput',
      'pushToHistoy',
      'setHistory',
      'setNotFound',
      'setDorme',
      'setGeneral'
    ]),
    onEnter () {
      // normalizing states
      this.setHistory(false)
      this.setNotFound(false)
      this.setDorme(false)
      this.setGeneral(false)
      if (this.input !== '') {
        this.setInput(this.input.toLowerCase())
        this.pushToHistoy(this.input.toLowerCase())
        if (this.knowCommands.includes(this.input.toLowerCase())) {
          switch (this.input.toLowerCase()) {
            case 'clear':
              this.input = ''
              break
            case 'history':
              this.setHistory(true)
              break
            case 'dorme dorme':
              this.setDorme(true)
              break
            default:
              this.setGeneral(true)
          }
        } else {
          this.setNotFound(true)
        }
      }
      this.input = ''
    }
  }
})
</script>

<style lang="sass">
section
  margin: 1rem
  .line,
  .oldline
    display: flex
    flex-grow: 1
    margin-bottom: 7px
    span
      font-size: $text-size
    .input
      position: relative
      margin-left: 5px
      flex-grow: 1
      text-transform: lowercase
      i
        position: absolute
        width: 1px
        height: 100%
        background-color: $color-foreground
        left: 18px
        top: 0
        animation-name: blink
        animation-duration: 800ms
        animation-iteration-count: infinite
        opacity: 1
      input
        background-color: $color-background
        font-size: $text-size
        color: $color-foreground
        width: calc( 100% - 20px )
        &:focus + i
          display: none
    &.disabled
      .input
        input,
        i
          display: none
          opacity: 0

@media screen and (max-width: $mobile-break)
  section
    .line,
    .oldline
      span
        font-size: $text-size-mobile
      .input
        input
          font-size: $text-size-mobile

@keyframes blink
  from
    opacity: 1
  to
    opacity: 0
</style>
