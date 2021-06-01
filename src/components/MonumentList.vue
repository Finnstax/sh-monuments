<template>
  <ul
    role="list"
    class="grid grid-cols-2 gap-x-4 gap-y-8 sm:grid-cols-3 sm:gap-x-6 lg:grid-cols-4 xl:gap-x-8"
  >
    <li
      v-for="monument in monuments.slice(0, 100)"
      :key="monument.Objektnummer"
      class="relative"
    >
      <div
        class="group block w-full aspect-w-10 aspect-h-7 rounded-lg bg-gray-100 focus-within:ring-2 focus-within:ring-offset-2 focus-within:ring-offset-gray-100 focus-within:ring-indigo-500 overflow-hidden"
      >
        <img
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
            >View details for {{ monument.Bezeichnung }}</span
          >
        </button>
      </div>
      <p
        class="mt-2 block text-sm font-medium text-gray-900 truncate pointer-events-none"
      >
        {{ monument.Bezeichnung }}
      </p>
      <p
        class="block text-sm font-medium text-gray-500 pointer-events-none truncate"
      >
        {{ monument["Adresse-Lage"] }}
      </p>
      <p class="block text-sm font-medium text-gray-500 pointer-events-none">
        {{ monument.Kreis }}
      </p>
    </li>
  </ul>
</template>

<script>
const files = [
  {
    title: "IMG_4985.HEIC",
    size: "3.9 MB",
    source:
      "https://images.unsplash.com/photo-1582053433976-25c00369fc93?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=512&q=80",
  },
  // More files...
];

import monuments from "../data/denkmalliste.json";
export default {
  computed: {
    monumentsWithImage() {
      return monuments.filter((x) => x.FotoURL);
    },
  },
  setup() {
    console.log(monuments);
    return {
      files,
      monuments,
    };
  },
};
</script>