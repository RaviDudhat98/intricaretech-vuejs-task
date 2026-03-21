<script setup>
import { ref } from 'vue'

const props = defineProps({
  isDialogOpen: {
    type: Boolean,
    required: true
  }
})

const emit = defineEmits(['update:isDialogOpen', 'startWorkflow'])

const selectedWorkflow = ref('advanced')

const handleNext = () => {
  emit('startWorkflow', selectedWorkflow.value)
}

const handleClose = () => {
  emit('update:isDialogOpen', false)
}
</script>

<template>
  <VDialog :model-value="isDialogOpen" @update:model-value="$emit('update:isDialogOpen', $event)" max-width="600">
    <VCard>
      <VCardItem class="pb-3 border-b">
        <div class="d-flex justify-space-between align-start px-1 pt-2">
          <div>
            <VCardTitle class="text-h5">Select Workflow Mode</VCardTitle>
            <VCardSubtitle>Choose how you want your campaign to behave</VCardSubtitle>
          </div>
          <VBtn icon="tabler-x" variant="text" color="medium-emphasis" @click="handleClose" />
        </div>
      </VCardItem>

      <VCardText class="pt-4">
        <VRadioGroup v-model="selectedWorkflow" hide-details>
          <!-- Advanced Workflow Card -->
          <VCard variant="outlined" class="mb-4 workflow-card cursor-pointer"
            :class="{ 'border-primary': selectedWorkflow === 'advanced' }" @click="selectedWorkflow = 'advanced'">
            <VCardText class="d-flex align-start pa-4">
              <VRadio value="advanced" color="primary" class="mt-n1 mr-2" />
              <div class="flex-grow-1">
                <div class="d-flex align-center mb-1">
                  <h6 class="text-base font-weight-medium mb-0 mr-3">Advanced Workflow</h6>
                  <VChip color="success" size="small" variant="tonal">Recommended</VChip>
                </div>
                <p class="text-body-2 text-medium-emphasis mb-2">Best for high-volume outreach</p>
                <div class="d-flex gap-4 text-caption text-medium-emphasis">
                  <div class="d-flex align-center">
                    <VIcon size="10" class="mr-1">tabler-circle-filled</VIcon>Conditional logic
                  </div>
                  <div class="d-flex align-center">
                    <VIcon size="10" class="mr-1">tabler-circle-filled</VIcon>Multiple paths
                  </div>
                  <div class="d-flex align-center">
                    <VIcon size="10" class="mr-1">tabler-circle-filled</VIcon>More control
                  </div>
                </div>
              </div>
              <div class="workflow-illustration bg-grey-lighten-4 rounded pa-2 d-none d-sm-block ml-4 text-center">
                <VIcon size="40" color="primary">tabler-hierarchy</VIcon>
              </div>
            </VCardText>
          </VCard>

          <!-- Standard Workflow Card -->
          <VCard variant="outlined" class="workflow-card cursor-pointer"
            :class="{ 'border-primary': selectedWorkflow === 'standard' }" @click="selectedWorkflow = 'standard'">
            <VCardText class="d-flex align-start pa-4">
              <VRadio value="standard" color="primary" class="mt-n1 mr-2" />
              <div class="flex-grow-1">
                <div class="d-flex align-center mb-1">
                  <h6 class="text-base font-weight-medium mb-0">Standard Workflow</h6>
                </div>
                <p class="text-body-2 text-medium-emphasis mb-2">Best for beginners</p>
                <div class="d-flex gap-4 text-caption text-medium-emphasis">
                  <div class="d-flex align-center">
                    <VIcon size="10" class="mr-1">tabler-circle-filled</VIcon>Linear steps
                  </div>
                  <div class="d-flex align-center">
                    <VIcon size="10" class="mr-1">tabler-circle-filled</VIcon>No conditions
                  </div>
                  <div class="d-flex align-center">
                    <VIcon size="10" class="mr-1">tabler-circle-filled</VIcon>Easy Setup
                  </div>
                </div>
              </div>
              <div class="workflow-illustration bg-grey-lighten-4 rounded pa-2 d-none d-sm-block ml-4 text-center">
                <VIcon size="40" color="info">tabler-list</VIcon>
              </div>
            </VCardText>
          </VCard>
        </VRadioGroup>
      </VCardText>

      <VCardActions class="px-6 py-4 border-t d-flex justify-end gap-3">
        <VBtn color="secondary" variant="tonal" @click="handleClose">Close</VBtn>
        <VBtn color="primary" variant="elevated" @click="handleNext">Next</VBtn>
      </VCardActions>
    </VCard>
  </VDialog>
</template>

<style scoped>
.gap-3 { gap: 12px; }
.gap-4 { gap: 16px; }
.workflow-card { transition: all 0.2s ease-in-out; }
.workflow-card:hover { border-color: rgba(var(--v-theme-primary), 0.5); }
</style>
