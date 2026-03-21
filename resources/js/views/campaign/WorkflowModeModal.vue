<script setup>
import { useConfigStore } from '@/@core/stores/config'
import advancedWorkflow from '@images/advanced-workflow.png'
import standardWorkflow from '@images/standard-workflow.png'
import { ref } from 'vue'

const configStore = useConfigStore()

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
  <VDialog :model-value="isDialogOpen" @update:model-value="$emit('update:isDialogOpen', $event)" max-width="650">
    <VCard>
      <VCardItem class="py-2 border-b" :style="{ background: configStore.theme === 'light' ? '#F8F8F8' : '' }">
        <div class="d-flex justify-space-between align-start mt-1">
          <div>
            <VCardTitle class="text-h4 font-medium text-[#5E5873]" style="font-size: 18px;">Select Workflow Mode
            </VCardTitle>
            <VCardSubtitle style="font-size: 14px;" class="font-medium text-[#5E5873]">Choose how you want your campaign
              to behave
            </VCardSubtitle>
          </div>
          <VBtn icon="tabler-playstation-x" variant="text" color="medium-emphasis" @click="handleClose" />
        </div>
      </VCardItem>

      <VCardText class="px-12 py-6">
        <VRadioGroup v-model="selectedWorkflow" hide-details>
          <!-- Advanced Workflow Card -->
          <VCard variant="outlined" class="mb-4 workflow-card cursor-pointer"
            :style="{ borderColor: selectedWorkflow === 'advanced' && configStore.theme === 'light' ? '#CFDAFE' : '', background: selectedWorkflow === 'advanced' && configStore.theme === 'light' ? '#F6F8FF' : '' }"
            @click="selectedWorkflow = 'advanced'">
            <VCardText class="d-flex align-start pa-4">
              <VRadio value="advanced" color="#3666EE" class="mt-n1 mr-0" />
              <div class="flex-grow-1">
                <div class="d-flex align-center mb-1">
                  <p class="text-base font-weight-bold mb-0 mr-3">Advanced Workflow</p>
                  <VChip color="#28C76F" size="small" variant="tonal" class="rounded-xl"
                    style="font-size: 12px; background: rgba(40, 199, 111, 0.12)">
                    Recommended</VChip>
                </div>
                <p class="text-body-2 text-medium-emphasis mb-2 text-[#5E5873]" style="">Best for high-volume outreach
                </p>
                <div class="d-flex gap-4 text-caption text-medium-emphasis text-[#5E5873]">
                  <div class="d-flex align-center">
                    <VIcon size="10" color="#B1B1B1" class="mr-1">tabler-circle-filled</VIcon>Conditional logic
                  </div>
                  <div class="d-flex align-center">
                    <VIcon size="10" color="#B1B1B1" class="mr-1">tabler-circle-filled</VIcon>Multiple paths
                  </div>
                  <div class="d-flex align-center">
                    <VIcon size="10" color="#B1B1B1" class="mr-1">tabler-circle-filled</VIcon>More control
                  </div>
                </div>
              </div>
              <div class="workflow-illustration d-none d-sm-flex align-center justify-center ml-4 flex-shrink-0"
                style="width: 80px;">
                <VImg :src="advancedWorkflow" width="80" />
              </div>
            </VCardText>
          </VCard>

          <!-- Standard Workflow Card -->
          <VCard variant="outlined" class="workflow-card cursor-pointer"
            :style="{ borderColor: selectedWorkflow === 'standard' && configStore.theme === 'light' ? '#CFDAFE' : '', background: selectedWorkflow === 'standard' && configStore.theme === 'light' ? '#F6F8FF' : '' }"
            @click="selectedWorkflow = 'standard'">
            <VCardText class="d-flex align-start pa-4">
              <VRadio value="standard" color="#3666EE" class="mt-n1 mr-2 flex-grow-0" />
              <div class="flex-grow-1">
                <div class="d-flex align-center mb-1">
                  <p class="text-base font-weight-bold mb-0">Standard Workflow</p>
                </div>
                <p class="text-body-2 text-medium-emphasis text-[#5E5873] mb-2">Best for beginners</p>
                <div class="d-flex gap-4 text-caption text-medium-emphasis text-[#5E5873]">
                  <div class="d-flex align-center">
                    <VIcon size="10" color="#B1B1B1" class="mr-1">tabler-circle-filled</VIcon>Linear steps
                  </div>
                  <div class="d-flex align-center">
                    <VIcon size="10" color="#B1B1B1" class="mr-1">tabler-circle-filled</VIcon>No conditions
                  </div>
                  <div class="d-flex align-center">
                    <VIcon size="10" color="#B1B1B1" class="mr-1">tabler-circle-filled</VIcon>Easy Setup
                  </div>
                </div>
              </div>
              <div class="workflow-illustration d-none d-sm-flex align-center justify-center ml-4 flex-shrink-0"
                style="width: 80px;">
                <VImg :src="standardWorkflow" width="80" />
              </div>
            </VCardText>
          </VCard>
        </VRadioGroup>
      </VCardText>

      <VCardActions class="px-6 pb-4 d-flex justify-end gap-3">
        <VBtn color="#9692A4" variant="tonal" @click="handleClose">Close</VBtn>
        <VBtn style="background: linear-gradient(239.27deg, #8BA6FF -27.06%, #3762EE 83.4%);" variant="elevated"
          @click="handleNext">Next</VBtn>
      </VCardActions>
    </VCard>
  </VDialog>
</template>

<style scoped>
.gap-3 {
  gap: 12px;
}

.gap-4 {
  gap: 16px;
}

.workflow-card {
  transition: all 0.2s ease-in-out;
  border-radius: 8px;
}

.workflow-card:hover {
  border-color: rgba(var(--v-theme-primary), 0.5);
}
</style>
