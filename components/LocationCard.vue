<template>
  <div class="location-item mb-6">
    <div
      class="surf-title w-full pt-8 pb-8 pl-6 pr-6"
      :style="{
        backgroundImage: 'url(' + singleSurf.cameras[0].stillUrl + ')',
      }"
    >
      <h3 class="text-4xl z-50 relative font-medium flex-grow">
        {{ singleSurf.name }}
      </h3>
      <a
        :href="singleSurf.cameras[0].streamUrl"
        class="relative z-50 inline-block text-center bg-primarybrand font-sans text-white text-base font-semibold py-3 px-6 rounded-full mt-4"
        target="_blank"
        >View cam
      </a>
    </div>
    <div
      class="surf-stats mb-4 p-6 flex flex-row flex-wrap flex-1 justify-start justify-items-stretch content-evenly"
    >
      <div class="stat-wrapper surf-height flex items-start">
        <div class="icon pt-1">
          <img src="@/assets/images/wave-ic.svg" />
        </div>
        <div class="flex flex-col justify-start pl-6">
          <div class="stat-content">
            {{ singleSurf.waveHeight.min }}-{{ singleSurf.waveHeight.max }}
            <span class="unit">ft</span>
            <span v-if="singleSurf.waveHeight.plus">+</span>
          </div>
          <div v-if="singleSurf.waveHeight.human" class="stat-description">
            {{ singleSurf.waveHeight.humanRelation }}
          </div>
        </div>
      </div>

      <div class="stat-wrapper surf-height flex items-start">
        <div class="icon pt-1">
          <img src="@/assets/images/wind-ic.svg" />
        </div>
        <div class="flex flex-col justify-start pl-6">
          <div class="stat-content">
            {{ singleSurf.wind.speed }}
            <span class="unit">kts</span>
          </div>
          <div class="stat-description">
            {{ degreeToCompass(singleSurf.wind.direction) }}
          </div>
        </div>
      </div>

      <div class="stat-wrapper tide flex items-start">
        <div class="icon pt-1">
          <img
            v-if="singleSurf.tide.next.type === 'HIGH'"
            src="@/assets/images/in-tide-ic.svg"
          />
          <img v-else src="@/assets/images/out-tide-ic.svg" />
        </div>
        <div class="flex flex-col justify-start pl-6">
          <div class="stat-content">
            {{ singleSurf.tide.current.height }}
            <span class="unit">m</span>
          </div>
          <div class="stat-description">
            {{ tideSummary(singleSurf.tide.next) }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'LocationCard',
  props: {
    location: {
      type: Object,
      required: true,
    },
    surfData: {
      type: Object,
      required: true,
    },
  },
  computed: {
    singleSurf() {
      console.log('running singlesurf')
      const spotId = this.location.id
      // console.log(spotId)
      // console.log(
      //   this.surfData.data.spots.filter((spot) => spot._id == spotId)[0]
      // )
      return this.surfData.data.spots.find((spot) => spot._id == spotId)
    },
  },
  methods: {
    degreeToCompass(value) {
      value = parseFloat(value)
      if (value <= 11.25) return 'N'
      value -= 11.25
      var allDirections = [
        'NNE',
        'NE',
        'ENE',
        'E',
        'ESE',
        'SE',
        'SSE',
        'S',
        'SSW',
        'SW',
        'WSW',
        'W',
        'WNW',
        'NW',
        'NNW',
        'N',
      ]
      var dIndex = parseInt(value / 22.5)
      return allDirections[dIndex] ? allDirections[dIndex] : 'N'
    },
    tideSummary(next) {
      const localTime = function (time) {
        const date = new Date(time * 1000)
        return new Intl.DateTimeFormat('en-AU', {
          hour12: true,
          hour: 'numeric',
          minute: 'numeric',
        }).format(date)
      }
      if (next.type == 'HIGH') {
        return `High tide ${next.height}m at ${localTime(next.timestamp)}`
      } else if (next.type == 'LOW') {
        return `High tide ${next.height}m at ${localTime(next.timestamp)}`
      }
    },
  },
}
</script>

<style lang="postcss">
.location-item {
  background-color: #faf9f2;
  border-radius: 8px;
}

.location-item {
  max-width: 600px;
  width: 100%;
}

.surf-title {
  position: relative;
  font-family: 'Young Serif';
  @apply bg-center;
  @apply bg-cover;
  @apply bg-no-repeat;
  border-radius: 8px 8px 0px 0px;

  &:before {
    content: '';
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    background-color: #000000;
    opacity: 0.4;
    border-radius: 8px 8px 0px 0px;
  }
}

.stat-wrapper {
  @apply mb-5;
  @apply w-full;
  @apply pr-0;
  @apply text-forestgreen;
  &:last-child {
    padding-right: 0;
    margin-bottom: 0;
  }
}

.stat-content {
  @apply font-extrabold text-2xl;
}

.stat-description {
  @apply text-sm font-semibold text-gray-600;
}
</style>
