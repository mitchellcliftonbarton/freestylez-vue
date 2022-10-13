<template>
  <div class="container">
    <div v-if="gotSupport" class="got-support">
      <!-- <router-link :to="this.$route.path === '/' ? '/info' : '/'" class="info-link">{{ this.$route.path === '/' ? '?' : 'X' }}</router-link> -->
      <Beats :isSafari="isSafari"></Beats>
      <FreestyleBody :isSafari="isSafari"></FreestyleBody>
      <div class="divide"></div>
      <router-view></router-view>
    </div>
    <div v-else class="no-support">
      <p>Sorry! Your browser/device doesn't support some of the libraries used to make this site, please try another browser or view this on desktop!</p>
    </div>
  </div>
</template>

<script>
import Beats from './Beats'
import FreestyleBody from './FreestyleBody'
import Modernizr from 'modernizr'

export default {
  name: 'Home',
  components: {
    Beats,
    FreestyleBody
  },
  data () {
    return {
      gotSupport: null,
      isSafari: null
    }
  },
  computed: {
    isMobile () {
      if (window.matchMedia('(min-width: 992px)').matches) {
        return false
      } else {
        return true
      }
    }
  },
  mounted () {
    if ((Modernizr.speechsynthesis && Modernizr.audio) && !this.isMobile) {
      this.gotSupport = true
    } else {
      this.gotSupport = false
    }

    if (/^((?!chrome|android).)*safari/i.test(navigator.userAgent)) {
      this.isSafari = true
    } else {
      this.isSafari = false
    }
  }
}
</script>

<style lang="scss">
  @import './../assets/scss/mixins/mixins';
  html {
    height : 100%;
  }

  body {
    height     : 100%;
    margin     : 0;
    // background : #343235;
    background-color: #000;

    .divide {
      position   : fixed;
      height     : 100%;
      width      : 1px;
      top        : 0px;
      background : #423f44;
      left       : 50%;
      display    : none;

      @include breakpoint(sm-up) {
        display: block;
      }
    }

    .info-link {
      color           : white;
      position        : fixed;
      top             : 20px;
      right           : 40px;
      text-decoration : none;
      // font-family     : 'VT323', monospace;
      font-family: 'Menlo';
      background      : red;
      padding         : 10px;
      z-index: 20;

      @include breakpoint(sm-up) {
        // background : #343235
        background-color: #000;
      }
    }

    .no-support {
      display         : flex;
      position        : fixed;
      width           : 100%;
      height          : 100%;
      top             : 0px;
      left            : 0px;
      justify-content : center;
      align-items     : center;

      p {
        width          : 80%;
        color          : #ded4d4;
        // font-family    : 'VT323', monospace;
        font-family: 'Menlo';
        letter-spacing : 0.1em;
        font-size      : 30px;

        @include breakpoint(sm-up) {
          width: 50%;
        }
      }
    }
  }

</style>
