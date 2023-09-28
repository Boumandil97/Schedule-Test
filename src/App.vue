<template>
  <div class="w-full flex items-center justify-center h-screen">
    <ScheduleTable />
  </div>
</template>

<script>
import ScheduleTable from "./components/ScheduleTable.vue";

export default {
  name: "App",
  components: {
    ScheduleTable,
  },
};
</script>

<style></style>
