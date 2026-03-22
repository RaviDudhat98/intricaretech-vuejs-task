<script setup>
import { ref } from 'vue'
import LookalikeAudienceModal from './LookalikeAudienceModal.vue'

const emit = defineEmits(['goBack', 'submit'])

const currentStep = ref(1) // 1 = Define Target Audience, 2 = Sender Profiles

const selectedImportMethod = ref('')

// CSV Mapping logic
const csvState = ref('upload') // 'upload' | 'mapping'

// Lookalike Modal logic
const isLookalikeModalOpen = ref(false)

// Sender Profiles logic
const currentProfileTab = ref('linkedin')

const selectImportMethod = (method) => {
  selectedImportMethod.value = selectedImportMethod.value === method ? '' : method
  if (method === 'lead') {
    isLookalikeModalOpen.value = true
  }
}

const handleNext = () => {
  if (currentStep.value === 1) {
    if (selectedImportMethod.value === 'csv' && csvState.value === 'upload') {
      csvState.value = 'mapping'
    } else {
      currentStep.value = 2
    }
  }
}

const handlePrevious = () => {
  if (currentStep.value === 2) {
    currentStep.value = 1
  } else if (currentStep.value === 1 && selectedImportMethod.value === 'csv' && csvState.value === 'mapping') {
    csvState.value = 'upload'
  }
}

const handleListSelected = (listName) => {
  console.log('Selected Lookalike List:', listName)
}
</script>

<template>
  <div class="advance-setup-view">
    <!-- Horizontal Stepper area -->
    <VCard class="mb-8 stepper-header-card border-none" elevation="0">
      <VCardText class="d-flex flex-column   pa-4">
        <div class="d-flex align-center px-6 py-3 border rounded">
          <div
            :class="['d-flex align-center  font-weight-medium cursor-pointer', currentStep >= 1 ? 'text-newPrimary' : 'text-disabled']"
            @click="currentStep = 1">
            <VAvatar :color="currentStep >= 1 ? 'newPrimary' : 'secondary'"
              :variant="currentStep >= 1 ? 'elevated' : 'tonal'" rounded size="38" class="mr-3">
              <VIcon size="22" v-if="currentStep > 1">tabler-check</VIcon>
              <VIcon size="22" v-else>tabler-list-details</VIcon>
            </VAvatar>
            <span class="text-subtitle-1 text-textPrimary font-weight-medium">Define Target Audience</span>
          </div>

          <VIcon size="20" class="mx-6 text-disabled text-textPrimary font-weight-medium">tabler-chevron-right</VIcon>

          <div
            :class="['d-flex align-center font-weight-medium', currentStep === 2 ? 'text-newPrimary' : 'text-disabled']">
            <VAvatar :color="currentStep === 2 ? 'newPrimary' : '#F1F5F9'"
              :variant="currentStep === 2 ? 'elevated' : 'flat'" rounded size="38" class="mr-3">
              <VIcon size="22" :color="currentStep === 2 ? 'white' : 'disabled'">tabler-user</VIcon>
            </VAvatar>
            <span class="text-subtitle-1">Sender Profiles</span>
          </div>
        </div>
        <!-- </VCardText> -->
        <!-- </VCard> -->

        <!-- Step 1 Content: Vertical Stepper -->
        <div v-if="currentStep === 1" class="vertical-stepper-container my-5">
          <!-- Sub-step 1: Choose Import Method -->
          <div class="stepper-item">
            <div class="stepper-indicator">
              <div class="stepper-dot" :class="{ 'completed': selectedImportMethod }">
                <VIcon v-if="selectedImportMethod" size="14" color="white">tabler-check</VIcon>
              </div>
              <div class="stepper-line"></div>
            </div>

            <div class="stepper-content">
              <!-- <VCard elevation="0" class="mb-6 border"> -->
              <VCardItem class="px-6 py-3 border rounded">
                <template #append>
                  <VIcon icon="tabler-chevron-up" size="20" class="text-textPrimary" />
                </template>
                <VCardTitle class="text-h6 text-textPrimary font-weight-medium">Choose Import Method
                  <span
                    class="mx-2 bg-newSecodary py-1 px-3 rounded text-bodyText font-weight-bold text-body-small">Step
                    1
                    of
                    2</span>
                </VCardTitle>
              </VCardItem>

              <VCardText class="pa-6">
                <VRow>
                  <!-- LinkedIn Search -->
                  <VCol cols="12" md="3">
                    <VCard variant="outlined" class="cursor-pointer h-100 import-card"
                      :class="{ 'selected': selectedImportMethod === 'linkedin' }"
                      @click="selectImportMethod('linkedin')">
                      <div class="pa-4">
                        <div class="d-flex justify-space-between align-start mb-3">
                          <VIcon size="28" color="newPrimary">tabler-brand-linkedin</VIcon>
                          <VCheckboxBtn v-if="selectedImportMethod === 'linkedin'" class="flex-grow-0"
                            :model-value="selectedImportMethod === 'linkedin'" color="newPrimary" density="compact" />
                        </div>
                        <h6 class="text-subtitle-1 font-weight-bold mb-1">LinkedIn Search</h6>
                        <p class="text-caption text-medium-emphasis mb-0">
                          (Basic, Sales Nav, Post, Group or Event URL)
                        </p>
                      </div>
                    </VCard>
                  </VCol>

                  <!-- Upload CSV -->
                  <VCol cols="12" md="3">
                    <VCard variant="outlined" class="cursor-pointer h-100 import-card"
                      :class="{ 'selected': selectedImportMethod === 'csv' }" @click="selectImportMethod('csv')">
                      <div class="pa-4">
                        <div class="d-flex justify-space-between align-start mb-3">
                          <VIcon size="28" color="newPrimary">tabler-calendar-event</VIcon>
                          <VCheckboxBtn v-if="selectedImportMethod === 'csv'" class="flex-grow-0"
                            :model-value="selectedImportMethod === 'csv'" color="newPrimary" density="compact" />
                        </div>
                        <h6 class="text-subtitle-1 font-weight-bold mb-1">Upload CSV File</h6>
                        <p class="text-caption text-medium-emphasis mb-0">
                          Upload LinkedIn profiles via CSV. <a href="#"
                            class="text-newPrimary text-decoration-none">Download
                            Sample</a>
                        </p>
                      </div>
                    </VCard>
                  </VCol>

                  <!-- Lookalike Audience -->
                  <VCol cols="12" md="3">
                    <VCard variant="outlined" class="cursor-pointer h-100 import-card"
                      :class="{ 'selected': selectedImportMethod === 'lead' }" @click="selectImportMethod('lead')">
                      <div class="pa-4">
                        <div class="d-flex justify-space-between align-start mb-3">
                          <VIcon size="28" color="newPrimary">tabler-user-plus</VIcon>
                          <VCheckboxBtn v-if="selectedImportMethod === 'lead'" class="flex-grow-0"
                            :model-value="selectedImportMethod === 'lead'" color="newPrimary" density="compact" />
                        </div>
                        <h6 class="text-subtitle-1 font-weight-bold mb-1">Lookalike
                          Audience</h6>
                        <p class="text-caption text-medium-emphasis mb-0">
                          Use Lead Finder to find audience.
                        </p>
                      </div>
                    </VCard>
                  </VCol>

                  <!-- Inbound Webhook -->
                  <VCol cols="12" md="3">
                    <VCard variant="outlined" class="cursor-pointer h-100 import-card"
                      :class="{ 'selected': selectedImportMethod === 'webhook' }"
                      @click="selectImportMethod('webhook')">
                      <div class="pa-4">
                        <div class="d-flex justify-space-between align-start mb-3">
                          <VIcon size="28" color="newPrimary">tabler-brand-linkedin</VIcon>
                          <VCheckboxBtn v-if="selectedImportMethod === 'webhook'" class="flex-grow-0"
                            :model-value="selectedImportMethod === 'webhook'" color="newPrimary" density="compact" />
                        </div>
                        <h6 class="text-subtitle-1 font-weight-bold mb-1">Inbound Webhook</h6>
                        <p class="text-caption text-medium-emphasis mb-0">
                          Sync leads from zapier, n8n make in real time
                        </p>
                      </div>
                    </VCard>
                  </VCol>
                </VRow>
              </VCardText>

            </div>
          </div>

          <!-- Sub-step 2: Dynamic Section based on Selected Method -->
          <div v-if="selectedImportMethod" class="stepper-item">
            <div class="stepper-indicator">
              <div class="stepper-dot active"></div>
            </div>

            <div class="stepper-content">
              <!-- Section: LinkedIn -->
              <VCard v-if="selectedImportMethod === 'linkedin'" elevation="0" class="mb-6 border">
                <VCardItem class="py-4 px-6">
                  <VCardTitle class="text-h6 font-weight-medium text-textPrimary">Paste LinkedIn Search URL
                  </VCardTitle>
                </VCardItem>
                <VDivider />
                <VCardText class="pa-6">
                  <div class="d-flex align-center bg-grey-lighten-4 rounded text-body-2 mb-4">
                    <VIcon size="20" color="newPrimary" class="mr-3">tabler-brand-linkedin</VIcon>
                    <div class="text-medium-emphasis">
                      Find your target audience with <a href="#"
                        class="text-decoration-underline text-newPrimary font-weight-medium">LinkedIn
                        Search</a>
                      or
                      <a href="#" class="text-newPrimary font-weight-medium text-decoration-underline">Sales
                        Navigator</a> or <a href="#"
                        class="text-newPrimary font-weight-medium text-decoration-underline">Post
                        URL</a> or <a href="#"
                        class="text-newPrimary font-weight-medium text-decoration-underline">Group URL</a>
                    </div>
                    <VSpacer />
                    <a href="#"
                      class="text-newPrimary text-decoration-underline d-flex align-center text-caption font-weight-medium">
                      <VIcon size="16" class="mr-1">tabler-help-circle</VIcon> Search Guide
                    </a>
                  </div>

                  <div class="d-flex align-center">
                    <VTextField placeholder="https://www.linkedin.com/search/results/people/?keywords="
                      variant="outlined" density="comfortable" hide-details class="mr-4 h-38" bg-color="white" />
                    <VBtn color="bgPrimary" elevation="0">Validate</VBtn>
                  </div>
                  <p class="text-caption text-medium-emphasis mt-3 mb-0 d-flex align-center">
                    <VIcon size="8" color="newPrimary" class="mr-2">tabler-circle-filled</VIcon>
                    Paste the search URL directly from LinkedIn
                  </p>
                </VCardText>
              </VCard>

              <!-- Section: CSV Upload -->
              <div v-if="selectedImportMethod === 'csv'">
                <!-- CSV Upload Box -->
                <VCard v-if="csvState === 'upload'" elevation="0" class="mb-6 border">
                  <VCardItem class="py-4 px-6">
                    <VCardTitle class="text-h6 font-weight-medium text-high-emphasis">Upload CSV File
                      <span
                        class="mx-2 bg-newSecodary py-1 px-3 rounded text-bodyText font-weight-bold text-body-small">Step
                        1
                        of
                        2</span>
                    </VCardTitle>
                  </VCardItem>
                  <VDivider />
                  <VCardText class="pa-6">
                    <div
                      class="border-dashed border-newPrimary rounded d-flex flex-column align-center justify-center pa-10 bg-grey-lighten-5 cursor-pointer csv-dropzone"
                      @click="csvState = 'mapping'">
                      <VAvatar color="newPrimary" variant="tonal" rounded size="48" class="mb-3">
                        <VIcon size="28">tabler-upload</VIcon>
                      </VAvatar>
                      <div class="text-newPrimary font-weight-medium mb-1">Drag a File or click to browse</div>
                      <div class="text-body-2 text-medium-emphasis">File with up to 100 rows works best</div>
                    </div>
                    <div class="mt-4">
                      <a href="#"
                        class="text-newPrimary text-decoration-none text-body-2 d-flex align-center font-weight-medium">
                        <VIcon size="18" class="mr-1">tabler-cloud-download</VIcon> Download a sample file
                      </a>
                    </div>
                  </VCardText>
                </VCard>

                <!-- CSV Mapping view -->
                <div v-else>
                  <VCard class="mb-4 border" elevation="0">
                    <VCardText class="d-flex align-center justify-space-between py-3">
                      <div class="d-flex align-center text-body-1">
                        <VIcon color="success" size="20" class="mr-2">tabler-circle-check-filled</VIcon>
                        <span class="font-weight-medium mr-4">Upload CSV file Selected</span>
                        <VChip size="small" variant="tonal" color="default">Step 1 of 2</VChip>
                      </div>
                      <VIcon size="20" class="text-medium-emphasis">tabler-chevron-down</VIcon>
                    </VCardText>
                  </VCard>
                  <VCard class="mb-4 border" elevation="0">
                    <VCardText class="d-flex align-center justify-space-between py-3">
                      <div class="d-flex align-center text-body-1">
                        <VIcon color="success" size="20" class="mr-2">tabler-circle-check-filled</VIcon>
                        <span class="font-weight-medium mr-4">Upload CSV File</span>
                        <VChip size="small" variant="tonal" color="default">Step 1 of 2</VChip>
                      </div>
                      <VIcon size="20" class="text-medium-emphasis">tabler-chevron-down</VIcon>
                    </VCardText>
                  </VCard>

                  <VCard class="mb-6 border" elevation="0">
                    <VCardItem class="pb-2 pt-5">
                      <div class="d-flex justify-space-between align-start">
                        <div>
                          <VCardTitle class="text-h5 text-textSecondary mb-1">Map Properties</VCardTitle>
                          <div class="d-flex align-center text-body-2 text-textSecondary">
                            <VIcon size="16" class="mr-1">tabler-check</VIcon> Make sure file includes contact name and
                            phone
                            number
                          </div>
                        </div>
                        <VBtn icon="tabler-trash" variant="text" color="error" />
                      </div>
                    </VCardItem>

                    <VCardText class="pa-6">
                      <VRow>
                        <VCol cols="12" md="8">
                          <VCard variant="outlined" class="h-100 bg-grey-lighten-5 border">
                            <VCardItem class="pb-2 pt-4">
                              <div class="d-flex align-center px-4">
                                <div style="flex: 1" class="text-textPrimary font-weight-semibold">Contact Field</div>
                                <div style="flex: 1" class="ml-4 text-textPrimary font-weight-semibold">CSV Column</div>
                              </div>
                            </VCardItem>
                            <VDivider />
                            <VCardText class="pa-4">
                              <div
                                v-for="(field, i) in ['Full name', 'First name', 'Last name', 'Company Name', 'Position', 'Headline']"
                                :key="i" class="d-flex align-center mb-3">
                                <VCard variant="outlined"
                                  class="flex-grow-1 px-4 py-2 d-flex align-center text-body-2 border">
                                  <VIcon size="18" class="text-success mr-3">tabler-list-details</VIcon> {{ field }}
                                </VCard>
                                <VCard variant="outlined"
                                  class="flex-grow-1 ml-4 px-4 py-2 d-flex align-center justify-space-between text-body-2 border">
                                  <div class="d-flex align-center">
                                    <VIcon size="18" class="text-medium-emphasis mr-3">tabler-user</VIcon> {{ field }}
                                  </div>
                                  <span class="text-medium-emphasis">(35)</span>
                                </VCard>
                              </div>
                            </VCardText>
                          </VCard>
                        </VCol>

                        <VCol cols="12" md="4">
                          <VCard variant="outlined" class="h-100 bg-grey-lighten-5 border">
                            <VCardItem class="pb-2 pt-4">
                              <VCardTitle class="text-subtitle-1 font-weight-semibold text-textPrimary">Unmapped Works
                              </VCardTitle>
                            </VCardItem>
                            <VDivider />

                            <VCardText class="pa-4">
                              <VTextField placeholder="Search" prepend-inner-icon="tabler-search" density="compact"
                                variant="outlined" class="mb-4 bg-white h-30" hide-details />
                              <div v-for="item in ['Location', 'Industry', 'Notes']" :key="item" class="mb-3">
                                <VCard variant="outlined"
                                  class="px-4 py-2 d-flex align-center justify-space-between bg-bgLight text-body-2 border">
                                  <div class="d-flex align-center">
                                    <VIcon size="18" class="text-medium-emphasis mr-2">tabler-list</VIcon> {{ item }}
                                    (9)
                                  </div>
                                  <span class="text-medium-emphasis">(3)</span>
                                </VCard>
                              </div>
                              <div class="text-right mt-4">
                                <a href="#"
                                  class="text-newPrimary text-body-2 text-decoration-none font-weight-medium">Clear
                                  All
                                  Matched</a>
                              </div>
                            </VCardText>
                          </VCard>
                        </VCol>
                      </VRow>
                    </VCardText>
                  </VCard>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Step 2 Content -->
        <div v-if="currentStep === 2">
          <div class="d-flex my-6 border-newPrimary flex-grow-0 ">
            <div
              class="cursor-pointer  border py-2 px-8 rounded-s text-subtitle-2 font-weight-medium border-newPrimary!"
              :class="currentProfileTab === 'linkedin' ? 'bg-activebg text-newPrimary ' : 'bg-white text-medium-emphasis'"
              @click="currentProfileTab = 'linkedin'" style="transition: all 0.2s ease">
              LinkedIn Profile
            </div>
            <div
              class="cursor-pointer border border-s-0 py-2 px-8 rounded-e text-subtitle-2 font-weight-medium border-newPrimary!"
              :class="currentProfileTab === 'email' ? 'bg-activebg text-newPrimary ' : 'bg-white text-medium-emphasis'"
              @click="currentProfileTab = 'email'" style="transition: all 0.2s ease">
              Email Accounts
            </div>
          </div>

          <VCard v-if="currentProfileTab === 'linkedin'" class="mb-6 border" elevation="0">
            <VCardItem class="pa-5 bg-white border-b">
              <div class="d-flex justify-space-between align-center">
                <div>
                  <div class="d-flex align-center text-h6 font-weight-medium text-high-emphasis">
                    <VIcon size="24" class="mr-3 bg-newPrimary">tabler-brand-linkedin</VIcon> LinkedIn Profile
                  </div>
                  <div class="text-body-2 text-medium-emphasis mt-2">Pick which LinkedIn profiles you want to use for
                    this
                    campaign.</div>
                </div>
                <VBtn color="newPrimary" prepend-icon="tabler-plus" elevation="0">
                  Add Account
                </VBtn>
              </div>
            </VCardItem>

            <!-- Table Controls -->
            <VCardText class="d-flex justify-space-between align-center py-4 px-5 bg-white">
              <div class="d-flex align-center text-body-2 text-medium-emphasis">
                Show
                <span class="mx-2 d-flex align-center cursor-pointer text-high-emphasis font-weight-bold">
                  10 <VIcon size="16" class="ml-1">tabler-chevron-down</VIcon>
                </span>
              </div>
              <VTextField placeholder="Search" prepend-inner-icon="tabler-search" density="compact" variant="outlined"
                style="max-width: 260px" hide-details bg-color="white" />
            </VCardText>

            <VDivider />

            <VCardText class="pa-0">
              <VTable class="text-no-wrap bg-white">

                <thead class="bg-tableHeadBg">
                  <tr>
                    <th style="width: 50px">
                      <VCheckboxBtn color="newPrimary" />
                    </th>
                    <th class="text-subtitle-2 font-weight-bold text-high-emphasis">NAME</th>
                    <th class="text-center text-subtitle-2 font-weight-bold text-high-emphasis">HEALTH</th>
                    <th class="text-center text-subtitle-2 font-weight-bold text-high-emphasis">DAILY LIMITS</th>
                    <th class="text-subtitle-2 font-weight-bold text-high-emphasis">ACCOUNT TYPE</th>
                    <th class="text-subtitle-2 font-weight-bold text-high-emphasis">STATUS</th>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <td>
                      <VCheckboxBtn color="newPrimary" />
                    </td>
                    <td>
                      <div class="d-flex align-center py-3">
                        <VAvatar size="40" class="mr-3 border">
                          <VImg src="https://i.pravatar.cc/150?img=11" />
                        </VAvatar>
                        <div class="d-flex flex-column">
                          <span class="text-subtitle-2 font-weight-bold text-high-emphasis">Edgar Jones</span>
                          <span class="text-caption text-medium-emphasis">1,250 connections</span>
                        </div>
                      </div>
                    </td>
                    <td class="text-center">
                      <VProgressCircular :model-value="72" color="warning" size="36" width="3">
                        <span class="text-caption font-weight-bold">72</span>
                      </VProgressCircular>
                    </td>
                    <td class="text-center">
                      <VChip variant="outlined" size="small" class="bg-white border">Invites: 40 / day</VChip>
                    </td>
                    <td>
                      <div class="d-flex align-center text-body-2">
                        <VIcon color="warning" size="20" class="mr-2">tabler-brand-linkedin</VIcon> Premium
                      </div>
                    </td>
                    <td>
                      <VChip color="success" size="small" class="rounded-pill" variant="flat" label>Connected</VChip>
                    </td>
                  </tr>
                </tbody>
              </VTable>
            </VCardText>
          </VCard>

          <VCard v-if="currentProfileTab === 'email'" class="mb-6 border" elevation="0">
            <VCardText class="d-flex justify-center align-center py-12 text-medium-emphasis">
              Email Accounts view coming soon...
            </VCardText>
          </VCard>
        </div>

        <!-- Footer Buttons -->
        <div class="d-flex justify-end mt-8">
          <div>
            <VBtn
              v-if="currentStep === 2 || (currentStep === 1 && selectedImportMethod === 'csv' && csvState === 'mapping')"
              variant="text" color="newPrimary" @click="handlePrevious">
              <VIcon start>tabler-arrow-left</VIcon> Previous
            </VBtn>
          </div>
          <div>
            <VBtn v-if="currentStep === 1" color="bgPrimary" elevation="0" @click="handleNext">
              Next</VBtn>
            <VBtn v-else color="bgPrimary" elevation="0" @click="$emit('submit')">Submit
            </VBtn>
          </div>
        </div>

        <!-- Lookalike Modal -->
        <LookalikeAudienceModal v-model:isDialogOpen="isLookalikeModalOpen" @listSelected="handleListSelected" />
      </VCardText>
    </VCard>
  </div>
</template>

<style scoped>
.stepper-header-card {
  border-radius: 8px !important;
}

.vertical-stepper-container {
  padding-left: 8px;
}

.stepper-item {
  display: flex;
  position: relative;
  gap: 24px;
}

.stepper-indicator {
  display: flex;
  flex-direction: column;
  align-items: center;
  position: relative;
  width: 24px;
}

.stepper-dot {
  width: 20px;
  height: 20px;
  border-radius: 50%;
  border: 2px solid #3666EE;
  background-color: white;
  z-index: 2;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 14px;
}

.stepper-dot.completed {
  background-color: #28C76F;
  border-color: #28C76F;
}

.stepper-dot.active {
  border-color: rgb(var(--v-theme-newPrimary));
  background-color: white;
}

.stepper-dot.active::after {
  content: '';
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background-color: rgb(var(--v-theme-newPrimary));
}

.stepper-line {
  position: absolute;
  top: 34px;
  bottom: -14px;
  width: 3px;
  background-color: #EAEFFF;
  z-index: 1;
}

.stepper-content {
  flex: 1;
}

.import-card {
  transition: all 0.2s ease-in-out;
  border-color: #E2E8F0 !important;
  border-width: 1.5px !important;
}

.import-card:hover {
  border-color: rgba(var(--v-theme-newPrimary), 0.5) !important;
  background-color: rgba(var(--v-theme-newPrimary), 0.02) !important;
}

.import-card.selected {
  border-color: rgb(var(--v-theme-newPrimary)) !important;
  background-color: rgba(var(--v-theme-newPrimary), 0.02) !important;
}

.border-dashed {
  border-style: dashed !important;
  border-width: 2px !important;
  border-color: #3666EE;
  background-color: rgba(var(--v-theme-newPrimary), 0.02) !important;
}

.border-none {
  border: none !important;
}

.border {
  border: 1.5px solid #E2E8F0 !important;
}
</style>
