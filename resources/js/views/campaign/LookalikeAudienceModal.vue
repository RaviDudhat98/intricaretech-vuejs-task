<script setup>
import { useConfigStore } from '@/@core/stores/config'
import { ref } from 'vue'

const props = defineProps({
  isDialogOpen: {
    type: Boolean,
    required: true
  }
})
const configStore = useConfigStore()

const emit = defineEmits(['update:isDialogOpen', 'listSelected'])

const hasLeads = ref(false)
const selectedLookalikeList = ref(null)

const handleClose = () => {
  emit('update:isDialogOpen', false)
}

const handleSelect = () => {
  emit('listSelected', selectedLookalikeList.value)
  handleClose()
}
</script>

<template>
  <VDialog :model-value="isDialogOpen" @update:model-value="$emit('update:isDialogOpen', $event)" max-width="500">
    <VCard>
      <VCardItem class="py-3 px-6 border-b" :style="{ background: configStore.theme === 'light' ? '#F8F8F8' : '' }">
        <div class="d-flex justify-space-between align-start pt-2 px-1">
          <div>
            <VCardTitle class="text-h5">Lookalikes</VCardTitle>
            <VCardSubtitle>Select a lookalike list for this campaign</VCardSubtitle>
          </div>
          <VBtn icon="tabler-x" variant="text" color="medium-emphasis" @click="handleClose" />
        </div>
      </VCardItem>

      <!-- Empty State -->
      <VCardText v-if="!hasLeads" class="d-flex flex-column justify-center align-center py-10">
        <h4 class="text-h6 text-newPrimary mb-1 mt-6">You don't have any leads</h4>
        <p class="text-body-2 text-medium-emphasis mb-6">Create a lead list to start running campaigns</p>
        <VBtn
          :style="{ background: configStore.theme === 'light' ? 'linear-gradient(239.27deg, #8BA6FF -27.06%, #3762EE 83.4%)' : '' }"
          @click="hasLeads = true" class="mb-4">Create a List</VBtn>
      </VCardText>

      <!-- Populated State -->
      <div v-else>
        <VCardText class="pt-6 pb-2">
          <!-- List 1 -->
          <VCard variant="outlined" class="mb-3 cursor-pointer workflow-card"
            :class="{ 'border-newPrimary bg-newPrimary-lighten-5': selectedLookalikeList === 'founder' }"
            @click="selectedLookalikeList = 'founder'">
            <VCardText class="d-flex align-center justify-space-between py-3 px-4 bg-bgLight">
              <div class="d-flex align-center">
                <VIcon size="20" class="mr-3 text-medium-emphasis">tabler-list</VIcon>
                <span class="text-body-1 font-weight-semibold text-textPrimary mr-2">Founder</span>
                <span class="text-body-2 text-textPrimary">(1000+ Users in the List)</span>
              </div>
              <VCheckboxBtn :model-value="selectedLookalikeList === 'founder'" color="newPrimary" class="flex-grow-0"
                v-if="selectedLookalikeList === 'founder'" />
            </VCardText>
          </VCard>

          <!-- List 2 -->
          <VCard variant="outlined" class="mb-2 cursor-pointer workflow-card"
            :class="{ 'border-newPrimary bg-newPrimary-lighten-5': selectedLookalikeList === 'tech' }"
            @click="selectedLookalikeList = 'tech'">
            <VCardText class="d-flex align-center justify-space-between py-3 px-4 bg-bgLight">
              <div class="d-flex align-center">
                <VIcon size="20" class="mr-3 text-medium-emphasis">tabler-list</VIcon>
                <span class="text-body-1 font-weight-semibold text-textPrimary mr-2">Tech Profiles</span>
                <span class="text-body-2 text-textPrimary">(1000+ Users in the List)</span>
              </div>
              <VCheckboxBtn :model-value="selectedLookalikeList === 'tech'" color="newPrimary" class="flex-grow-0"
                v-if="selectedLookalikeList === 'tech'" />
            </VCardText>
          </VCard>

          <div class="text-right mt-3 mb-2">
            <a href="#" class="text-newPrimary text-body-2 text-decoration-none">Add New</a>
          </div>
        </VCardText>

        <VCardActions class="px-6 py-4 border-t d-flex justify-end gap-3">
          <VBtn color="secondary" variant="tonal" @click="handleClose">Cancel</VBtn>
          <VBtn color="newPrimary" variant="elevated" @click="handleSelect">Select List</VBtn>
        </VCardActions>
      </div>
    </VCard>
  </VDialog>
</template>

<style scoped>
.gap-3 {
  gap: 12px;
}

.workflow-card {
  transition: all 0.2s ease-in-out;
}

.workflow-card:hover {
  border-color: rgba(var(--v-theme-newPrimary), 0.5);
}
</style>
