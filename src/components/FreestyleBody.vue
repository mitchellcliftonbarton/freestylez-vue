<template>
    <div class="freestyle-body">
        <div v-if="!clicked" @click="turnOn()" class="click-start" ref="clickToStart" id="click-start">
            <p>ENCORE! ENCORE! ENCORE!</p>
            <p>CLICK TO START</p>
            <p class="sub">(Works best in Chrome)</p>
        </div>
        <div class="main" id="main" ref="main">
            <p class="load-text">v1.3</p>
            <p class="load-text">LOADING</p>
            <p class="load-text">LOADING</p>
            <p class="load-text">LOADING</p>
            <p class="load-text">LOADING</p>
            <p class="load-text">CRANK UP THE VOLUME</p>
            <p class="load-text">COMING AT YOU LIVE</p>
        </div>
        <div v-if="this.switching" class="switch">
            <div class="in">PASSING THE MIC!</div>
        </div>
    </div>
</template>

<script>
/* eslint-disable */
import words from './../content/words'
import { bus } from './../consts.js'

export default {
  name: 'FreestyleBody',
  data () {
    return {
      synth         : window.speechSynthesis,
      names         : [
        'Alex',
        'Agnes',
        'Kathy',
        'Bruce',
        'Fred',
        'Junior',
        'Princess',
        'Vicki',
        'Ralph',
        'Victoria'
      ],
      rapperz       : [],
      currentRapper : 0,
      barLen        : [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
      swTm          : [30000, 45000, 40000, 20000, 15000, 25000],
      timez         : [500, 750, 1000, 1500, 1750],
      who           : null,
      spit          : null,
      currentBar    : 0,
      limitBar      : Math.floor(Math.random() * (10 - 6 + 1) + 6),
      left          : true,
      starting      : null,
      switching     : false,
      clicked       : false
    }
  },
  computed: {
    wordz () {
      return words
    },
    allWordz () {
      let x = []

      for (var variable in this.wordz) {
        x.push(variable)
      }

      return x
    },
    lines () {
      return this.$refs.main
    }
  },
  methods: {
    turnOn () {
      this.clicked ? this.clicked = false : this.clicked = true
      this.loadVoices()
    },
    voicesFunc (array) {
      for (let i = 0; i < array.length; i++) {
        for (let x = 0; x < this.names.length; x++) {
          if (this.names[x] === array[i].name) {
            this.rapperz.push(array[i])
          }
        }
      }

      this.who = this.rapperz[this.currentRapper]
      this.loadingText()
    },
    loadVoices () {
      let voices

      if (this.synth.onvoiceschanged !== undefined) {
        voices = this.synth.getVoices()

        this.synth.onvoiceschanged = this.voicesFunc(voices)
      } else {
        voices = this.synth.getVoices()

        this.voicesFunc(voices)
      }
    },
    loadingText () {
      let loadingTexts = [...document.getElementsByClassName('load-text')]
      let time = 400

      loadingTexts.forEach((el, index) => {
        setTimeout(() => {
          el.classList.add('visible')
          if (index === (loadingTexts.length - 1)) this.hypeIt()
        }, time)

        time = time + 400
      })
    },
    hypeIt () {
      this.starting = new SpeechSynthesisUtterance(this.wordz.hype[Math.floor(Math.random() * this.wordz.hype.length)])
      this.synth.speak(this.starting)

      this.starting.onend = () => {
        setTimeout(() => {
          bus.$emit('startRap')
          this.printRapper()
          this.rap()
        }, 1000)
      }
    },
    printRapper () {
        if (this.left) {
            var node = document.createElement('p')
            node.classList.add('name')
            let textnode = document.createTextNode(`-------- RAPPER: ${this.rapperz[this.currentRapper].name} --------`)
            node.appendChild(textnode)
            this.lines.appendChild(node)
        } else {
            var node2 = document.createElement('p')
            node2.classList.add('right', 'name')
            let textnode2 = document.createTextNode(`-------- RAPPER: ${this.rapperz[this.currentRapper].name} --------`)
            node2.appendChild(textnode2)
            this.lines.appendChild(node2)
        }
    },
    rap () {
      let rando = this.barLen[Math.floor(Math.random() * this.barLen.length)]

      let rando2
      let rando3
      let bar = ''

      for (let i = 0; i < rando; i++) {
        let x = Math.floor(Math.random() * this.allWordz.length)
        let y = this.allWordz[x]

        // get random array of words
        rando2 = this.wordz[y]
        // get random value in the random array
        rando3 = rando2[Math.floor(Math.random() * rando2.length)]
        bar += rando3 + ' '
      }

      if (this.currentBar >= this.limitBar) {
        this.currentBar = 0
        this.limitBar = Math.floor(Math.random() * (15 - 6 + 1) + 6)
        this.choozRapper()
      } else {
        this.currentBar++
      }

      this.spit = new SpeechSynthesisUtterance()
      this.spit.text = bar
      this.spit.voice = this.who
      this.synth.speak(this.spit)

      this.spit.onend = () => {
        if (this.left === true) {
          var node = document.createElement('p')
          let textnode = document.createTextNode(bar)
          node.appendChild(textnode)
          this.lines.appendChild(node)
        } else {
          var node2 = document.createElement('p')
          node2.className = 'right'
          let textnode2 = document.createTextNode(bar)
          node2.appendChild(textnode2)
          this.lines.appendChild(node2)
        }

        window.scrollTo(0, document.body.scrollHeight)

        setTimeout(() => {
          this.rap()
        }, this.timez[Math.floor(Math.random() * this.timez.length)])
      }
    },
    choozRapper () {
      this.left ? this.left = false : this.left = true

      this.switching = true

      setTimeout(() => {
        this.switching = false
      }, 700)

      // dont pick the current rapper
      let nextRapper = Math.floor(Math.random() * this.rapperz.length)

      while (nextRapper === this.currentRapper) {
        nextRapper = Math.floor(Math.random() * this.rapperz.length)
      }

      this.currentRapper = nextRapper

      this.who = this.rapperz[this.currentRapper]
      this.printRapper()
      console.log(this.who)
    },
    mounted () {
      bus.$on('new-beat', beat => {
        console.log(beat)
      })
    }
  }
}
</script>

<style lang="scss">
@import './../assets/scss/mixins/mixins';

.freestyle-body {
  .click-start {
    position: absolute;
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    // background: #343235;
    background-color: #000;
    z-index: 10;
    flex-direction: column;
  }

  .click-start,
  .main {
    p {
      color          : #ded4d4;
      font-family: 'Menlo';
      // font-family    : 'VT323', monospace;
      // letter-spacing : 0.1em;
      // font-size      : 30px;
      font-size: 14px;

      &.sub {
        font-size: 14px;
      }
    }
  }

  .main {
    width   : 100%;
    height  : 100%;
    padding : 60px 0px 20px 0px;

    @include breakpoint(sm-up) {
      padding : 20px 0px;
    }

    p {
      width          : 90%;
      padding        : 0px 5%;
      margin         : 14px 0px;

      @include breakpoint(sm-up) {
        width   : 46%;
        padding : 0 2%;
      }

      &.beat-line {
        width: 100%;
        padding: 0;
        text-align: center;
        color: #384eff;
      }
    }

    .right {
      // color : #888896;
      color: #fff;

      @include breakpoint(sm-up) {
        padding : 0 2% 0 52%;
      }
    }

    .name {
        color: red;
    }

    .load-text {
      opacity: 0;
      color: #00ffc6;

      &.visible {
        opacity: 1;
      }
    }
  }

  .switch {
    position       : fixed;
    top            : 43%;
    width          : 100%;
    text-align     : center;
    z-index        : 100;
    pointer-events : none;

    .in {
      width          : 250px;
      margin         : 0 auto;
      // font-family    : 'VT323', monospace;
      font-family: 'Menlo';
      background     : red;
      padding        : 5px;
      color          : black;
      font-size      : 14px;
      letter-spacing : 4px;
    }
  }
}

</style>
