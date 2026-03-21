<script setup>
import { ref, onMounted } from 'vue'

// Import split components
import EmptyStateView from '../views/campaign/EmptyStateView.vue'
import WorkflowModeModal from '../views/campaign/WorkflowModeModal.vue'
import AdvanceCampaignSetup from '../views/campaign/AdvanceCampaignSetup.vue'
import StandardCampaignSetup from '../views/campaign/StandardCampaignSetup.vue'

const currentView = ref('empty')
const isMounted = ref(false)

onMounted(() => {
  isMounted.value = true
})

// Modals State
const isWorkflowModalOpen = ref(false)

const startWorkflow = (workflowType) => {
  isWorkflowModalOpen.value = false
  if (workflowType === 'advanced') {
    currentView.value = 'advance'
  } else if (workflowType === 'standard') {
    currentView.value = 'standard'
  }
}

const handleLaunch = () => {
  // Normally some API sumbit here
  currentView.value = 'empty'
}
</script>

<template>
  <div class="campaign-page">
    <Teleport to="#navbar-breadcrumbs" v-if="isMounted">
      <div v-if="currentView === 'empty'" class="d-flex align-center">
        <VIcon size="18" color="primary" class="cursor-pointer">tabler-home</VIcon>
        <VIcon size="16" class="mx-2 text-medium-emphasis">tabler-chevron-right</VIcon>
        <span class="text-body-2 text-medium-emphasis font-weight-medium">Campaign</span>
      </div>
      <div v-if="currentView === 'advance'" class="d-flex align-center">
        <span class="text-body-2 text-primary cursor-pointer font-weight-medium" @click="currentView = 'empty'">Campaign</span>
        <VIcon size="16" class="mx-2 text-medium-emphasis">tabler-chevron-right</VIcon>
        <span class="text-body-2 text-high-emphasis font-weight-medium">Advance Campaign</span>
      </div>
      <div v-if="currentView === 'standard'" class="d-flex align-center">
        <span class="text-body-2 text-primary cursor-pointer font-weight-medium" @click="currentView = 'empty'">Campaign</span>
        <VIcon size="16" class="mx-2 text-medium-emphasis">tabler-chevron-right</VIcon>
        <span class="text-body-2 text-high-emphasis font-weight-medium">Standard Campaign</span>
      </div>
    </Teleport>

    <!-- Sub-components rendered based on view state -->
    <EmptyStateView v-if="currentView === 'empty'" @openWorkflowModal="isWorkflowModalOpen = true" />

    <AdvanceCampaignSetup v-else-if="currentView === 'advance'" @goBack="currentView = 'empty'" @submit="handleLaunch" />

    <StandardCampaignSetup v-else-if="currentView === 'standard'" @goBack="currentView = 'empty'" @submit="handleLaunch" />

    <!-- Modals -->
    <WorkflowModeModal v-model:isDialogOpen="isWorkflowModalOpen" @startWorkflow="startWorkflow" />
  </div>
</template>
