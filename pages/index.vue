<template>
  <div>
    <div
      class="header container mx-auto flex flex-col justify-center items-center py-8"
    >
      <img src="@/assets/images/logo.svg" />
    </div>
    <div class="container mx-auto px-4 flex flex-col items-center pb-20">
      <LocationCard
        v-for="location in locations"
        :key="location.title"
        :location="location"
        :surf-data="surfData"
      />
    </div>
  </div>
</template>

<script>
import LocationCard from '~/components/LocationCard'
import locationData from '~/data/locations.json'
export default {
  components: {
    LocationCard,
  },
  async asyncData({ $http }) {
    const surfData = await $http.$get(
      'https://services.surfline.com/kbyg/regions/overview?subregionId=58581a836630e24c44879024'
    )

    console.log(surfData)

    return { surfData }
  },
  data() {
    return {
      locations: locationData,
      surfData: [],
    }
  },
}
</script>

<style>
/* Sample `apply` at-rules with Tailwind CSS
.container {
@apply min-h-screen flex justify-center items-center text-center mx-auto;
}
*/

.header {
  color: white;

  img {
    width: 240px;
  }
}
</style>
