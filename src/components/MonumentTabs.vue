<template>
  <div class="pb-6">
    <div class="sm:hidden">
      <label for="tabs" class="sr-only">Tab aussuchen</label>
      <select
        id="tabs"
        name="tabs"
        class="block w-full pl-3 pr-10 py-2 text-base border-gray-300 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm rounded-md"
      >
        <option
          v-for="(reason, index) in reasons"
          :key="index"
          :selected="reason.active"
        >
          {{ reason.label }} {{ reason.count }}
        </option>
      </select>
    </div>
    <div class="hidden sm:block">
      <div class="border-b border-gray-200">
        <nav class="-mb-px flex flex-wrap gap-x-5" aria-label="Tabs">
          <a
            v-for="(reason, index) in reasons"
            :key="index"
            @click="updateActive(index, reason.label)"
            href="#"
            :class="[
              reason.active
                ? 'border-indigo-500 text-indigo-600'
                : 'border-transparent text-gray-500 hover:text-gray-700 hover:border-gray-200',
              'whitespace-nowrap flex py-4 px-1 border-b-2 font-medium text-sm',
            ]"
            :aria-current="reason.active ? 'page' : undefined"
          >
            {{ reason.label }}

            <span
              v-if="reason.count"
              :class="[
                true
                  ? 'bg-indigo-100 text-indigo-600'
                  : 'bg-gray-100 text-gray-900',
                'hidden ml-2 py-0.5 px-2.5 rounded-full text-xs font-medium md:inline-block',
              ]"
              >{{ reason.count }}</span
            >
          </a>
        </nav>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    updateActive(index, label) {
      this.$emit("updateActive", label);
    },
  },
  props: ["reasons"],
  setup() {
    return {};
  },
  data() {
    return {};
  },
};
</script>