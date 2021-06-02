<template>
  <div v-if="field">
    <dt class="text-sm font-medium text-gray-500 sm:w-40 sm:flex-shrink-0">
      {{ label }}
    </dt>
    <dd class="mt-1 text-sm text-gray-900 flex flex-wrap gap-2">
      <span
        v-for="(item, index) in field"
        :key="index"
        class="inline-flex items-center px-3 py-1.5 rounded-full text-xs font-medium bg-gray-100 text-gray-800"
      >
        {{ item }}
      </span>
    </dd>
  </div>
</template>

<script>
export default {
  components: {},
  props: ["label", "field"],
  setup() {},
};
</script>