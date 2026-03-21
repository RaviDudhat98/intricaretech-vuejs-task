<script setup>
const emit = defineEmits(['openWorkflowModal'])
</script>

<template>
  <div class="empty-state-view">
    <!-- Top controls -->
    <VRow class="mb-6 align-center">
      <VCol cols="12" md="3">
        <VSelect style="min-height: 38px;" label="All" :items="['All', 'Active', 'Draft']" density="compact"
          variant="outlined" hide-details />
      </VCol>
      <VCol cols="12" md="4">
        <VTextField style="min-height: 38px;" placeholder="Search" prepend-inner-icon="tabler-search"
          density="compact" variant="outlined" hide-details />
      </VCol>
    </VRow>

    <!-- Empty State Graphic -->
    <div class="d-flex flex-column align-center justify-center mt-10">
      <div class="empty-illustration mb-6 d-flex justify-center align-center">
        <VIcon size="120" color="primary" class="opacity-50">tabler-file-search</VIcon>
      </div>
      <VBtn style="background: linear-gradient(239.27deg, #8BA6FF -27.06%, #3762EE 83.4%) !important;"
        @click="$emit('openWorkflowModal')">
        New Campaign
      </VBtn>
    </div>
  </div>
</template>

<style scoped>
.v-input :deep(.v-field__input) {
  min-height: 38px !important;
}
</style>
