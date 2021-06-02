<!-- This example requires Tailwind CSS v2.0+ -->
<template>
  <TransitionRoot as="template" :show="isOpen">
    <Dialog
      as="div"
      static
      class="fixed inset-0 z-10 overflow-hidden"
      @close="
        isOpen = false;
        $emit('currentState', this.isOpen);
      "
      :open="isOpen"
    >
      <div class="absolute inset-0 overflow-hidden">
        <DialogOverlay class="absolute inset-0" />

        <div class="fixed inset-y-0 right-0 pl-10 max-w-full flex sm:pl-16">
          <TransitionChild
            as="template"
            enter="transform transition ease-in-out duration-500 sm:duration-700"
            enter-from="translate-x-full"
            enter-to="translate-x-0"
            leave="transform transition ease-in-out duration-500 sm:duration-700"
            leave-from="translate-x-0"
            leave-to="translate-x-full"
          >
            <div class="w-screen max-w-2xl">
              <div
                class="h-full flex flex-col bg-white shadow-xl overflow-y-scroll"
              >
                <div class="px-4 py-6 sm:px-6">
                  <div class="flex items-start justify-between">
                    <h2
                      id="slide-over-heading"
                      class="text-lg font-medium text-gray-900"
                    >
                      Details
                    </h2>
                    <div class="ml-3 h-7 flex items-center">
                      <button
                        class="bg-white rounded-md text-gray-400 hover:text-gray-500 focus:ring-2 focus:ring-indigo-500"
                        @click="
                          (isOpen = false), $emit('currentState', this.isOpen)
                        "
                      >
                        <span class="sr-only">Close panel</span>
                        <XIcon class="h-6 w-6" aria-hidden="true" />
                      </button>
                    </div>
                  </div>
                </div>
                <!-- Main -->
                <div>
                  <div class="pb-1 sm:pb-6">
                    <div>
                      <div class="relative h-40 sm:h-96">
                        <img
                          class="absolute h-full w-full object-cover"
                          v-if="monument.FotoURL"
                          :src="monument.FotoURL"
                          alt=""
                        />
                        <img
                          class="absolute h-full w-full object-cover"
                          v-if="!monument.FotoURL"
                          src="../assets/noimagefound.png"
                          alt=""
                        />
                      </div>
                      <div
                        class="mt-6 px-4 sm:mt-8 sm:flex sm:items-end sm:px-6"
                      >
                        <div class="sm:flex-1">
                          <div>
                            <div class="flex items-center">
                              <h3
                                class="font-bold text-xl text-gray-900 sm:text-2xl"
                              >
                                {{ monument.Bezeichnung }}
                              </h3>
                            </div>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                  <div class="px-4 pt-5 pb-5 sm:px-0 sm:pt-0">
                    <dl class="space-y-8 px-4 sm:px-6 sm:space-y-6">
                      <DetailFieldIterate
                        :label="'Schutzumfang'"
                        :field="monument.Schutzumfang"
                      />
                      <DetailFieldIterate
                        :label="'Begründung'"
                        :field="monument.Begründung"
                      />
                      <DetailField
                        :label="'Kulturdenkmaltyp'"
                        :field="monument.Kulturdenkmaltyp"
                      />
                      <DetailField
                        :label="'Adresse / Lage'"
                        :field="monument['Adresse-Lage']"
                      />
                      <DetailField :label="'Kreis'" :field="monument.Kreis" />
                      <DetailField
                        :label="'Gemeinde'"
                        :field="monument.Gemeinde"
                      />
                      <DetailField
                        :label="'Objektnummer'"
                        :field="monument.Objektnummer"
                      />
                    </dl>
                  </div>
                </div>
              </div>
            </div>
          </TransitionChild>
        </div>
      </div>
    </Dialog>
  </TransitionRoot>
</template>

<script>
import {
  Dialog,
  DialogOverlay,
  TransitionChild,
  TransitionRoot,
} from "@headlessui/vue";
import { XIcon } from "@heroicons/vue/outline";
import {} from "@heroicons/vue/solid";
import DetailField from "./DetailField.vue";
import DetailFieldIterate from "./DetailFieldIterate.vue";

export default {
  props: ["isActive", "detailData"],
  components: {
    Dialog,
    DialogOverlay,
    TransitionChild,
    TransitionRoot,
    XIcon,
    DetailField,
    DetailFieldIterate,
  },
  watch: {
    isActive() {
      this.isOpen = this.isActive;
      this.$emit("currentState", this.isOpen);
    },
    detailData() {
      this.monument = this.detailData;
    },
  },
  data() {
    return {
      isOpen: this.isActive,
      monument: this.detailData,
    };
  },
};
</script>