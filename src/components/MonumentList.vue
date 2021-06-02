<template>
  <MonumentTabs />

  <span v-for="(reason, index) in extractedReasoning" :key="index">
    {{ reason }}
  </span>
  <ul
    role="list"
    class="grid grid-cols-2 gap-x-4 gap-y-8 sm:grid-cols-3 sm:gap-x-6 lg:grid-cols-4 xl:gap-x-8"
  >
    <li
      v-for="monument in monumentsWithImage.slice(0, limit)"
      :key="monument.Objektnummer"
      class="relative"
    >
      <div
        class="group block w-full aspect-w-10 aspect-h-7 rounded-lg bg-gray-100 focus-within:ring-2 focus-within:ring-offset-2 focus-within:ring-offset-gray-100 focus-within:ring-indigo-500 overflow-hidden"
      >
        <img
          loading="lazy"
          v-if="monument.FotoURL"
          :src="monument.FotoURL"
          alt=""
          class="object-cover pointer-events-none group-hover:opacity-75"
        />
        <img
          v-if="!monument.FotoURL"
          src="../assets/noimagefound.png"
          alt=""
          class="object-cover pointer-events-none group-hover:opacity-75"
        />
        <button type="button" class="absolute inset-0 focus:outline-none">
          <span class="sr-only"
            >Details anzeigen für {{ monument.Bezeichnung }}</span
          >
        </button>
      </div>
      <p
        class="mt-2 block text-sm font-medium text-gray-900 truncate pointer-events-none"
      >
        {{ monument.Bezeichnung }}
      </p>
      <p class="block text-sm font-medium text-gray-500 pointer-events-none">
        {{ monument.Kreis }}
      </p>
    </li>
  </ul>
  <div class="flex justify-center pt-5">
    <button
      v-if="limit < monumentsWithImage.length"
      @click="limit += 50"
      type="button"
      class="center mt-2 inline-flex items-center px-4 py-2 border border-transparent shadow-sm text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
    >
      <PlusCircleIcon class="-ml-1 mr-2 h-5 w-5" aria-hidden="true" />
      Mehr anzeigen
    </button>
  </div>
</template>

<script>
import monuments from "../data/denkmalliste.json";
import MonumentTabs from "./MonumentTabs.vue";
import { PlusCircleIcon } from "@heroicons/vue/solid";

export default {
  components: {
    PlusCircleIcon,
    MonumentTabs,
  },
  data() {
    return {
      limit: 100,
    };
  },
  computed: {
    monumentsWithImage() {
      return monuments.filter((x) => x.FotoURL);
    },
    extractedReasoning() {
      let reasons = [];
      monuments.forEach((monument) => {
        if (monument["Begründung"]) {
          monument["Begründung"].forEach((element) => {
            reasons.indexOf(element) === -1 ? reasons.push(element) : "";
          });
        }
      });
      console.log(reasons);
      reasons = reasons.sort((a, b) => a - b);
      return reasons;
    },
  },
  setup() {
    return {
      monuments,
    };
  },
};
</script>