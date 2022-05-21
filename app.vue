<template>
  <div class="w-10/12 mx-auto">
    <div class="grid grid-cols-5 text-lg font-bold mt-10 gap-3">
      <div class="tab bg-gray-600">Panels</div>
      <div class="tab bg-gray-800">Consumption</div>
      <div class="tab bg-gray-800">Storage</div>
    </div>
    <div class="bg-gray-600 rounded-b-xl rounded-tr-xl p-5">
      <div class="grid grid-cols-6 gap-3">
        <div class="mb-4">
          <label class="" for="username"> Lat </label>
          <input
            class="focus:outline-none focus:shadow-outline"
            id="lat"
            type="number"
            placeholder="Latitude"
            :value="lat"
          />
        </div>
        <div class="mb-4">
          <label class="" for="username"> Lon </label>
          <input
            class="focus:outline-none focus:shadow-outline"
            id="long"
            type="number"
            placeholder="Longitude"
            :value="lon"
          />
        </div>
        <div v-if="solardata && solardata.inputs && solardata.inputs.location">
          {{ solardata.inputs.elevation }}m AMSL,
        </div>
        <div class="mb-4">
          <label class="" for="username"> Neigung </label>
          <input
            class="focus:outline-none focus:shadow-outline"
            id="long"
            type="number"
            placeholder="Longitude"
            :value="angle"
          />
        </div>
        <div class="mb-4">
          <label class="" for="username"> Richtung (Süd = 0) </label>
          <input
            class="focus:outline-none focus:shadow-outline"
            id="long"
            type="number"
            placeholder="Süd = 0°, Ost = -90°"
            :value="aspect"
          />
        </div>
        <div class="mb-4">
          <button
            type="button"
            class="py-2 px-4 flex justify-center items-center bg-blue-600 hover:bg-blue-700 focus:ring-blue-500 focus:ring-offset-blue-200 text-white w-full transition ease-in duration-200 text-center text-base font-semibold shadow-md focus:outline-none focus:ring-2 focus:ring-offset-2 w-12 h-12 rounded-lg"
          >
            Add PV System
          </button>
        </div>
      </div>

      <div
        v-if="solardata && solardata.status"
        class="bg-red-200 border-red-600 text-red-600 border-l-4 p-4"
        role="alert"
      >
        <p class="font-bold"># {{ solardata.status }}</p>
        <p>
          {{ solardata.message }}
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const lat = ref(50.086);
const lon = ref(9.081);
const angle = ref(50);
const aspect = ref(-5);

const solardata = await fetch(
  `https://re.jrc.ec.europa.eu/api/v5_2/seriescalc?lat=${lat.value}&lon=${lon.value}&raddatabase=PVGIS-SARAH2&browser=1&outputformat=json&userhorizon=&usehorizon=1&angle=${angle.value}&aspect=${aspect.value}&startyear=2020&endyear=2020&mountingplace=free&optimalinclination=0&optimalangles=0&js=1&select_database_hourly=PVGIS-SARAH2&hstartyear=2020&hendyear=2020&trackingtype=0&hourlyangle=${angle.value}&hourlyaspect=${aspect.value}&pvcalculation=1&pvtechchoice=crystSi&peakpower=1&loss=14&components=1`
).then((r) => r.json());
</script>

<style>
body {
  @apply text-white;
  background-image: url("https://images.unsplash.com/photo-1562408954-be39449c4962?crop=entropy&cs=tinysrgb&fm=jpg&ixlib=rb-1.2.1&q=80&raw_url=true&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1548");
  background-size: "contain";
}
label {
  @apply block text-gray-300 text-sm font-bold mb-2;
}
input {
  @apply shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight;
}
.tab {
  @apply rounded-t-xl  text-center p-2;
}
</style>
