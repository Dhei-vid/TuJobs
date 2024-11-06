<script setup>
import { RouterLink } from "vue-router";
import JobListing from "./JobListing.vue";
import PulseLoader from "vue-spinner/src/PulseLoader.vue";
import { reactive, ref, defineProps, onMounted } from "vue";
import axios from "axios";

// ref and reactive do the same thing except ref takes objects and primitive but reactive only takes objects
// const jobs = ref([]);

// using reactive
const state = reactive({
  jobs: [],
  isLoading: true,
});

defineProps({
  limit: Number,
  showBtn: {
    type: Boolean,
    default: false,
  },
});

onMounted(async () => {
  try {
    const response = await axios("/api/jobs");
    const data = response?.data;
    state.jobs = data;
  } catch (error) {
    console.log(error);
  } finally {
    state.isLoading = false;
  }
});
</script>

<template>
  <section class="bg-green-50 px-4 py-10">
    <div class="container-xl lg:container m-auto">
      <h2 class="text-3xl font-bold text-green-500 mb-6 text-center">
        Browse Jobs
      </h2>

      <!-- Show loading spinner while loading is true -->
      <div v-if="state.isLoading" class="text-center py-6 text-gray-500">
        <PulseLoader />
      </div>

      <!-- Show job listing when done loading -->
      <div v-else class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <JobListing
          v-for="job in state.jobs.slice(0, limit || state.jobs.length)"
          :key="job.id"
          :job="job"
        />
      </div>

      <section v-if="showBtn" class="m-auto max-w-lg my-10 px-6">
        <RouterLink
          to="jobs"
          class="block bg-black text-white text-center py-4 px-6 rounded-xl hover:bg-gray-700"
          >View All Jobs
        </RouterLink>
      </section>
    </div>
  </section>
</template>
