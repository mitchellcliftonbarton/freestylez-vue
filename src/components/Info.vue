<template>
  <div class="info">
    <div class="modal">
      <p>freestylez.mitchellbarton.com is a web adapted version of the piece 'F R E E S T Y L E Z ( B A T T L E )' by <a href="http://mitchellbarton.com/" target="_blank">Mitchell Barton</a>. The original piece exists as a desktop command line application for Mac OS and is available for download <a href="http://mitchellbarton.com/freestylez-app/freestylez-app.zip">here</a>.</p>
      <p>You can also view an installation of the piece <a href="https://youtu.be/7GQQV3D3NSI" target="_blank">here</a>.</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Info'
}
</script>

<style lang="scss">
  @import './../assets/scss/mixins/mixins';
  .info {
    position        : fixed;
    width           : 100%;
    height          : 100%;
    top             : 0px;
    left            : 0px;
    display         : flex;
    justify-content : center;
    align-items     : center;
    pointer-events  : none;
    z-index: 15;

    .modal {
      background     : #282629;
      width          : 70%;
      padding        : 30px;
      pointer-events : auto;

      @include breakpoint(sm-up) {
        width: 50%;
      }

      p {
        color          : white;
        // font-family    : 'VT323', monospace;
        font-family: 'Menlo';
        line-height    : 1.5em;
        // letter-spacing : .1em;
        font-size      : 14px;
        word-wrap      : break-word;

        @include breakpoint(sm-up) {
          word-wrap : initial;
        }
      }

      a {
        color : white;

        &:hover {
          color: purple;
        }
      }
    }
  }
</style>
