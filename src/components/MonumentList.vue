<template>
  <Toggle @toggleVisiblity="updateVisiblity" />
  <MonumentTabs @updateActive="updateActive" :reasons="extractedReasoning" />
  <ul
    role="list"
    class="grid grid-cols-2 gap-x-4 gap-y-8 sm:grid-cols-3 sm:gap-x-6 lg:grid-cols-4 xl:gap-x-8"
  >
    <li
      v-for="monument in filteredMonuments.slice(0, limit)"
      :key="monument.Objektnummer"
      class="relative rounded-b-lg bg-white"
    >
      <div
        class="group block w-full aspect-w-10 aspect-h-7 rounded-t-lg bg-gray-100 focus-within:ring-2 focus-within:ring-offset-2 focus-within:ring-offset-gray-100 focus-within:ring-indigo-500 overflow-hidden"
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
      <div class="p-4">
        <p
          class="block text-sm font-medium text-gray-900 truncate pointer-events-none"
        >
          {{ monument.Bezeichnung }}
        </p>
        <p class="block text-sm font-medium text-gray-500 pointer-events-none">
          {{ monument.Kreis }}
        </p>
        <button
          @click="(showSlideOver = true), (detailMonument = monument)"
          type="button"
          class="inline-flex items-center mt-2 px-2.5 py-1.5 border border-gray-300 shadow-sm text-xs font-medium rounded text-gray-700 bg-white hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
        >
          Details
        </button>
      </div>
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
      activeIndex: 0,
      searchReason: "Alle",
    };
  },
  methods: {
    updateState(data) {
      this.showSlideOver = data;
    },
    handleScroll() {
      let elm = document.getElementById("main");
      if (elm.scrollHeight - (elm.scrollTop + elm.clientHeight) < 100) {
        this.limit += 10;
      }
    },
    updateVisiblity(val) {
      this.showOnlyWithImage = val;
    },
    updateActive(label) {
      this.searchReason = label;
      console.log(label);
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
    filterForImage() {
      var filterMonuments = monuments;
      if (this.showOnlyWithImage) {
        filterMonuments = filterMonuments.filter((x) => x.FotoURL);
      }
      return filterMonuments;
    },
    filterForQuery() {
      var filterMonuments = this.filterForImage;
      return filterMonuments.filter(
        (x) =>
          x.Bezeichnung.toLowerCase().includes(
            this.searchQuery.toLowerCase()
          ) || x.Kreis.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
    },
    filteredMonuments() {
      var filterMonuments = this.filterForQuery;

      return filterMonuments
        .filter((x) => {
          if (this.searchReason == "Alle") {
            return true;
          }

          let reason = x["Begründung"];

          if (reason && reason.join().includes(this.searchReason)) {
            return true;
          }
        })
        .sort(function () {
          return 0.5 - Math.random();
        });
    },
    extractedReasoning() {
      var counts = {};
      counts["Alle"] = this.filterForQuery.length;
      this.filterForQuery.forEach((monument) => {
        if (monument["Begründung"]) {
          monument["Begründung"].forEach((element) => {
            counts[element] = counts[element] ? counts[element] + 1 : 1;
          });
        }
      });

      var retArr = [];
      var count = 0;

      Object.entries(counts).forEach(([key, value]) => {
        var obj = {
          label: key,
          count: value,
          active: key == this.searchReason ? true : false,
          index: count,
        };
        count++;
        retArr.push(obj);
      });

      return retArr.sort((a, b) =>
        a.label > b.label ? 1 : b.label > a.label ? -1 : 0
      );
    },
  },
  setup() {
    return {
      monuments,
    };
  },
};
</script>