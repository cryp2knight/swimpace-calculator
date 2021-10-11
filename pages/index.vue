<template>
  <v-container>
    <v-row>
      <v-col>
        <h2 class="grey--text text--darken-5">
          What do you want to calculate?
        </h2>
      </v-col>
    </v-row>
    <choices v-model="type" :items="[PACE, TIME, DISTANCE]" main />
    <choices
      v-show="type === PACE"
      v-model="paceMin"
      :items="['min/100yds', 'min/100m']"
    />
    <choices
      v-show="type === DISTANCE"
      v-model="distanceUnit"
      :items="['yards', 'meters']"
    />
    <time-input v-show="type && type !== TIME" v-model="timeInputs" />
    <distance-input
      v-show="type && type !== DISTANCE"
      v-model="distanceInputs"
    />
    <pace-input v-show="type && type !== PACE" v-model="paceInputs" />
    <v-row v-if="type" justify="center" style="margin-top: 2rem">
      <v-btn
        x-large
        color="success"
        style="font-weight: 600; font-size: 1.5rem"
        @click="calculate"
      >
        Calculate
      </v-btn>
    </v-row>
    <v-divider v-if="result" style="margin-top: 1rem" />
    <results v-if="result" :type="type" :result="result" />
  </v-container>
</template>

<script>
import DistanceInput from '~/components/DistanceInput.vue'
import PaceInput from '~/components/PaceInput.vue'
import Results from '~/components/Results.vue'
import TimeInput from '~/components/TimeInput.vue'

const m2y = (m) => {
  return m * 1.093613
}

const y2m = (y) => {
  return y / 1.093613
}

const paceMeter = 'min/100m'
const paceYard = 'min/100yds'

const conversion = {
  [paceMeter]: {
    require: 'meters',
    convert: y2m
  },
  [paceYard]: {
    require: 'yards',
    convert: m2y
  }
}

export default {
  components: { TimeInput, DistanceInput, PaceInput, Results },
  data () {
    return {
      PACE: 'PACE',
      TIME: 'TIME',
      DISTANCE: 'DISTANCE',
      type: '',
      paceMin: paceYard,
      distanceUnit: 'yards',
      timeInputs: {
        hours: 0,
        minutes: 0,
        seconds: 0
      },
      distanceInputs: {
        distance: 0,
        unit: 'yards'
      },
      paceInputs: {
        minutes: 0,
        seconds: 0,
        ms: 0,
        choice: paceYard
      },
      result: null
    }
  },
  watch: {
    type (prev, cur) {
      if (prev !== cur) {
        this.result = null
      }
    }
  },
  methods: {
    calculate () {
      switch (this.type) {
        case this.PACE:
          this.calcPace()
          break
        case this.TIME:
          this.calcTime()
          break
        case this.DISTANCE:
          this.calcDistance()
          break
        default:
          this.result = null
      }
    },
    calcPace () {
      const { seconds, minutes, hours } = this.timeInputs
      const totalSeconds = +seconds + (minutes * 60) + (hours * 3600)
      let distance = this.distanceInputs.distance
      if (this.distanceInputs.unit !== conversion[this.paceMin].require) {
        distance = conversion[this.paceMin].convert(distance)
      }
      const pace = totalSeconds * 100 / distance
      this.result = {
        minutes: Math.floor(pace / 60) || 0,
        seconds: Math.floor(pace % 60) || 0,
        ms: Math.round((pace % 60 - (Math.floor(pace % 60))) * 100) || 0
      }
    },
    calcTime () {
      let distance = this.distanceInputs.distance
      const { minutes, seconds, ms, choice } = this.paceInputs
      if (this.distanceInputs.unit !== conversion[choice].require) {
        distance = conversion[choice].convert(distance)
      }
      const pace = +seconds + (60 * minutes) + (+`.${ms.toString().trim()}`)

      const timeInSeconds = distance * pace / 100
      const rHours = Math.floor(timeInSeconds / 3600)
      const rMinutes = Math.floor((timeInSeconds - rHours * 3600) / 60)
      const rSeconds = Math.round(timeInSeconds - (rHours * 3600) - (rMinutes * 60))
      this.result = {
        hours: rHours || 0,
        minutes: rMinutes || 0,
        seconds: rSeconds || 0
      }
    },
    calcDistance () {
      const { minutes, seconds, ms, choice } = this.paceInputs
      const pace = +seconds + (60 * minutes) + (+`.${ms.toString().trim()}`)

      const totalSeconds = +this.timeInputs.seconds + (this.timeInputs.minutes * 60) + (this.timeInputs.hours * 3600)

      let distance = (totalSeconds * 100) / pace
      if (this.distanceUnit !== conversion[choice].require) {
        distance = this.distanceUnit === 'meters' ? y2m(distance) : m2y(distance)
      }
      this.result = {
        [this.distanceUnit]: (distance || 0).toFixed(2)
      }
    }
  }
}
</script>
