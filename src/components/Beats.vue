<template>
    <audio id="audio" ref="audioPlayer">
        <source :src="theBeats[currentBeat]" ref="audioSource" type="audio/mpeg">
    </audio>
</template>

<script>
/* eslint-disable */
import { bus } from './../consts.js'

export default {
  name: 'Beats',
  props: {
    isSafari: Boolean
  },
  data () {
    return {
      // beats: [
      //   'street-fighter-3.mp3',
      //   'street-fighter-1.mp3',
      //   'street-fighter-2.mp3',
      //   'final-fight-1.mp3',
      //   'final-fight-2.mp3',
      //   'final-fight-3.mp3',
      //   'final-fight-4.mp3',
      //   '1942-1.mp3',
      //   'bad-dudes-1.mp3',
      //   'r-type-1.mp3',
      //   'r-type-2.mp3',
      //   'tiger-heli-1.mp3',
      //   'twin-cobra-1.mp3',
      //   'up-n-down-1.mp3',
      //   'vigilante-1.mp3',
      //   'vigilante-2.mp3',
      //   'vs-castlevania-1.mp3',
      //   'choplifter.mp3',
      //   'golden-axe.mp3',
      //   'gunforce.mp3',
      //   'killer-instinct-1.mp3',
      //   'killer-instinct-2.mp3',
      //   'snow-bros.mp3',
      //   'street-fighter-4.mp3',
      //   'super-cobra.mp3',
      //   'wonder-boy.mp3'
      // ],
      beats: [
        '88.mp3',
        '93.mp3',
        'breathe.mp3',
        'cream.mp3',
        'drop.mp3',
        'electric.mp3',
        'everything.mp3',
        'far.mp3',
        'hard-2.mp3',
        'jazz.mp3',
        'ny.mp3',
        'preservation.mp3',
        'ready.mp3',
        'saliva.mp3',
        'vibe.mp3',
        'welcome.mp3',
        'what.mp3'
      ],
      currentBeat: 0
    }
  },
  computed: {
    theBeats () {
      let beats = []

      this.beats.forEach((x) => {
        let beat = require('./../assets/beats/' + x)

        beats.push(beat)
      })

      this.shuffle(beats)

      return beats
    }
  },
  methods: {
    shuffle (array) {
      let currentIndex = array.length
      let temporaryValue
      let randomIndex

      // While there remain elements to shuffle...
      while (currentIndex !== 0) {
        // Pick a remaining element...
        randomIndex = Math.floor(Math.random() * currentIndex)
        currentIndex -= 1

        // And swap it with the current element.
        temporaryValue = array[currentIndex]
        array[currentIndex] = array[randomIndex]
        array[randomIndex] = temporaryValue
      }

      return array
    },
    addBeatLine () {
      var node = document.createElement('p')
      node.classList.add('beat-line')
      let textnode = document.createTextNode(`-------- THE BEAT: ${this.theBeats[this.currentBeat]} --------`)
      node.appendChild(textnode)
      document.getElementById('main').appendChild(node)
    },
    run () {
      // console.log('running', this.theBeats[this.currentBeat])
      this.addBeatLine()
      this.$refs.audioPlayer.load()
      this.$refs.audioPlayer.play()
    }
  },
  mounted () {
    if (!this.isSafari) {
      bus.$on('startRap', () => {
        this.run()
      })
    } else {
      document.getElementById('click-start').addEventListener('click', () => {
        this.run()
      })
    }

    this.$refs.audioPlayer.addEventListener('ended', (e) => {
        this.currentBeat > this.theBeats.length
        ? this.currentBeat = 0
        : this.currentBeat++

        console.log('ended, moving to ' + this.currentBeat)
          this.run()
        })
  }
}
</script>
