<template>
  <Toggle @toggleVisiblity="updateVisiblity" />
  <MonumentTabs :reasons="extractedReasoning" />
  <ul
    role="list"
    class="grid grid-cols-2 gap-x-4 gap-y-8 sm:grid-cols-3 sm:gap-x-6 lg:grid-cols-4 xl:gap-x-8"
  >
    <li
      v-for="monument in filteredMonuments.slice(0, limit)"
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
        <button
          @click="(showSlideOver = true), (detailMonument = monument)"
          type="button"
          class="absolute inset-0 focus:outline-none"
        >
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
  <SlideOver
    :detailData="detailMonument"
    @currentState="updateState"
    :isActive="showSlideOver"
  />
  <div class="flex justify-center pt-5">
    <button
      v-if="limit < filteredMonuments.length"
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
import SlideOver from "./SlideOver.vue";
import Toggle from "./Toggle.vue";
import { PlusCircleIcon } from "@heroicons/vue/solid";

export default {
  props: ["searchQuery"],
  components: {
    PlusCircleIcon,
    MonumentTabs,
    Toggle,
    SlideOver,
  },
  data() {
    return {
      limit: 50,
      showOnlyWithImage: true,
      showSlideOver: false,
      detailMonument: "",
    };
  },
  methods: {
    updateState(data) {
      this.showSlideOver = data;
    },
    handleScroll() {
      let elm = document.getElementById("main");
      if (elm.scrollHeight - (elm.scrollTop + elm.clientHeight) < 300) {
        this.limit += 20;
      }
    },
    updateVisiblity(val) {
      this.showOnlyWithImage = val;
    },
  },
  mounted() {
    this.$nextTick(function () {
      document
        .getElementById("main")
        .addEventListener("scroll", this.handleScroll);
    });
  },
  beforeUnmount() {
    document
      .getElementById("main")
      .removeEventListener("scroll", this.handleScroll);
  },
  computed: {
    filteredMonuments() {
      var filterMonuments = monuments;
      if (this.showOnlyWithImage) {
        filterMonuments = filterMonuments.filter((x) => x.FotoURL);
      }
      return filterMonuments
        .filter(
          (x) =>
            x.Bezeichnung.toLowerCase().includes(
              this.searchQuery.toLowerCase()
            ) || x.Kreis.toLowerCase().includes(this.searchQuery.toLowerCase())
        )
        .sort(function () {
          return 0.5 - Math.random();
        });
    },
    extractedReasoning() {
      var counts = {};
      counts["Alle"] = this.filteredMonuments.length;
      this.filteredMonuments.forEach((monument) => {
        if (monument["Begründung"]) {
          monument["Begründung"].forEach((element) => {
            counts[element] = counts[element] ? counts[element] + 1 : 1;
          });
        }
      });

      return counts;
    },
  },
  setup() {
    return {
      monuments,
    };
  },
};
</script>