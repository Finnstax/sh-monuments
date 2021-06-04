<!-- This example requires Tailwind CSS v2.0+ -->
<template>
  <div
    class="mt-6 max-w-7xl mx-auto px-4 sm:px-6 md:px-8 flex-1 flex justify-between invisible hidden md:block md:visible"
  >
    <div class="rounded-md bg-yellow-50 p-4">
      <div class="flex">
        <div class="flex-shrink-0">
          <CheckCircleIcon class="h-5 w-5 text-yellow-400" aria-hidden="true" />
        </div>
        <div class="ml-3">
          <p class="text-sm font-medium text-yellow-800">
            Suchen Sie nach einer besonderen Art von Denkmälern, wie zum Beispiel Wassertürme, oder nach einem Monument in einer spezifischen Region?
            Nutzen Sie einfach die
            Suchfunktion.
          </p>
        </div>
        <div class="ml-auto pl-3">
          <div class="-mx-1.5 -my-1.5">
            <button
              type="button"
              class="inline-flex bg-yellow-50 rounded-md p-1.5 text-yellow-500 hover:bg-yellow-100 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-offset-yellow-50 focus:ring-yellow-600"
            >
              <span class="sr-only">Dismiss</span>
              <XIcon class="h-5 w-5" aria-hidden="true" />
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { CheckCircleIcon, XIcon } from "@heroicons/vue/solid";

export default {
  components: {
    CheckCircleIcon,
    XIcon,
  },
};
</script>