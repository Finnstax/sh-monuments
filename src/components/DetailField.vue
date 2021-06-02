<template>
  <div v-if="field">
    <dt class="text-sm font-medium text-gray-500 sm:w-40 sm:flex-shrink-0">
      {{ label }}
    </dt>
    <dd class="mt-1 text-sm text-gray-900 sm:col-span-2">
      <p>{{ field }}</p>
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