<script setup>
import { ref } from 'vue'
import LookalikeAudienceModal from './LookalikeAudienceModal.vue'

const emit = defineEmits(['goBack', 'submit'])

const currentStep = ref(1) // 1 = Define Target Audience, 2 = Sender Profiles

const selectedImportMethod = ref('linkedin')

// CSV Mapping logic
const csvState = ref('upload') // 'upload' | 'mapping'

// Lookalike Modal logic
const isLookalikeModalOpen = ref(false)

// Sender Profiles logic
const currentProfileTab = ref('linkedin')

const selectImportMethod = (method) => {
  selectedImportMethod.value = method
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
    <!-- Stepper area -->
    <VCard class="mb-6">
      <VCardText class="d-flex align-center pa-4">
        <div
          :class="['d-flex align-center font-weight-medium cursor-pointer', currentStep >= 1 ? 'text-primary' : 'text-disabled']"
          @click="currentStep = 1">
          <VAvatar :color="currentStep >= 1 ? 'primary' : 'secondary'"
            :variant="currentStep >= 1 ? 'elevated' : 'tonal'" rounded size="32" class="mr-2"
            :class="{ 'opacity-50': currentStep < 1 }">
            <VIcon size="20" v-if="currentStep > 1">tabler-check</VIcon>
            <VIcon size="20" v-else>tabler-users</VIcon>
          </VAvatar>
          Define Target Audience
        </div>
        <VIcon size="20" class="mx-4 text-disabled">tabler-chevron-right</VIcon>
        <div
          :class="['d-flex align-center font-weight-medium', currentStep === 2 ? 'text-primary' : 'text-disabled']">
          <VAvatar :color="currentStep === 2 ? 'primary' : 'secondary'"
            :variant="currentStep === 2 ? 'elevated' : 'tonal'" rounded size="32" class="mr-2"
            :class="{ 'opacity-50': currentStep !== 2 }">
            <VIcon size="20">tabler-user</VIcon>
          </VAvatar>
          Sender Profiles
        </div>
      </VCardText>
    </VCard>

    <!-- Step 1 Content -->
    <div v-if="currentStep === 1">
      <!-- Import Method Selection -->
      <VCard title="Choose Import Method" class="mb-6">
        <VCardText>
          <VRow>
            <!-- LinkedIn Search -->
            <VCol cols="12" md="3">
              <VCard variant="outlined" class="cursor-pointer h-100 import-card"
                :class="{ 'border-primary bg-primary-lighten-5': selectedImportMethod === 'linkedin' }"
                @click="selectImportMethod('linkedin')">
                <div class="pa-4">
                  <div class="d-flex justify-space-between align-start mb-2">
                    <VAvatar color="primary" variant="tonal" rounded size="40">
                      <VIcon size="24">tabler-brand-linkedin</VIcon>
                    </VAvatar>
                    <VCheckboxBtn :model-value="selectedImportMethod === 'linkedin'" color="primary" />
                  </div>
                  <h6 class="text-h6 mb-1">LinkedIn Search</h6>
                  <p class="text-body-2 text-medium-emphasis mb-0">
                    (Basic, Sales Nav, Post, Group or Event URL)
                  </p>
                </div>
              </VCard>
            </VCol>

            <!-- Upload CSV -->
            <VCol cols="12" md="3">
              <VCard variant="outlined" class="cursor-pointer h-100 import-card"
                :class="{ 'border-primary bg-primary-lighten-5': selectedImportMethod === 'csv' }"
                @click="selectImportMethod('csv')">
                <div class="pa-4">
                  <div class="d-flex justify-space-between align-start mb-2">
                    <VAvatar color="info" variant="tonal" rounded size="40">
                      <VIcon size="24">tabler-file-upload</VIcon>
                    </VAvatar>
                    <VCheckboxBtn :model-value="selectedImportMethod === 'csv'" color="primary" />
                  </div>
                  <h6 class="text-h6 mb-1">Upload CSV File</h6>
                  <p class="text-body-2 text-medium-emphasis mb-0">
                    Upload LinkedIn profiles via CSV. <a href="#" class="text-primary text-decoration-none">Download
                      Sample</a>
                  </p>
                </div>
              </VCard>
            </VCol>

            <!-- Lookalike Audience -->
            <VCol cols="12" md="3">
              <VCard variant="outlined" class="cursor-pointer h-100 import-card"
                :class="{ 'border-primary bg-primary-lighten-5': selectedImportMethod === 'lead' }"
                @click="selectImportMethod('lead')">
                <div class="pa-4">
                  <div class="d-flex justify-space-between align-start mb-2">
                    <VAvatar color="secondary" variant="tonal" rounded size="40">
                      <VIcon size="24">tabler-users-group</VIcon>
                    </VAvatar>
                    <VCheckboxBtn :model-value="selectedImportMethod === 'lead'" color="primary" />
                  </div>
                  <h6 class="text-h6 mb-1">Lookalike Audience</h6>
                  <p class="text-body-2 text-medium-emphasis mb-0">
                    Upload your best profiles, let AI find their lookalikes.
                  </p>
                </div>
              </VCard>
            </VCol>

            <!-- Inbound Webhook -->
            <VCol cols="12" md="3">
              <VCard variant="outlined" class="cursor-pointer h-100 import-card"
                :class="{ 'border-primary bg-primary-lighten-5': selectedImportMethod === 'webhook' }"
                @click="selectImportMethod('webhook')">
                <div class="pa-4">
                  <div class="d-flex justify-space-between align-start mb-2">
                    <VAvatar color="warning" variant="tonal" rounded size="40">
                      <VIcon size="24">tabler-plug-connected</VIcon>
                    </VAvatar>
                    <VCheckboxBtn :model-value="selectedImportMethod === 'webhook'" color="primary" />
                  </div>
                  <h6 class="text-h6 mb-1">Inbound Webhook</h6>
                  <p class="text-body-2 text-medium-emphasis mb-0">
                    Sync leads from zapier, n8n make in real time
                  </p>
                </div>
              </VCard>
            </VCol>
          </VRow>
        </VCardText>
      </VCard>

      <!-- Dynamic Section based on Selected Method -->
      <!-- Section: LinkedIn -->
      <VCard v-if="selectedImportMethod === 'linkedin'" title="Paste LinkedIn Search URL" class="mb-6">
        <VCardText>
          <div class="d-flex align-center bg-grey-lighten-4 rounded pa-4 text-body-2 mb-3">
            <VIcon size="20" color="primary" class="mr-2">tabler-brand-linkedin</VIcon>
            <span>Find your target audience with <a href="#" class="text-primary text-decoration-none">LinkedIn
                Search</a> or <a href="#" class="text-primary text-decoration-none">Sales Navigator</a> or <a href="#"
                class="text-primary text-decoration-none">Post URL</a> or <a href="#"
                class="text-primary text-decoration-none">Group URL</a></span>
            <VSpacer />
            <a href="#" class="text-primary text-decoration-none d-flex align-center">
              <VIcon size="16" class="mr-1">tabler-help-circle</VIcon> Search Guide
            </a>
          </div>

          <div class="d-flex">
            <VTextField placeholder="https://www.linkedin.com/search/results/people/?keywords=" variant="outlined"
              density="compact" hide-details class="mr-4" />
            <VBtn color="primary">Validate</VBtn>
          </div>
          <p class="text-caption text-medium-emphasis mt-2 mb-0 d-flex align-center">
            <VIcon size="10" color="primary" class="mr-1">tabler-circle-filled</VIcon> Paste the search URL directly
            from LinkedIn
          </p>
        </VCardText>
      </VCard>

      <!-- Section: CSV Upload -->
      <div v-if="selectedImportMethod === 'csv'">
        <!-- CSV Upload Box -->
        <VCard v-if="csvState === 'upload'" title="Upload CSV File" class="mb-6">
          <VCardText>
            <div
              class="border-dashed border-primary rounded d-flex flex-column align-center justify-center pa-10 bg-primary-lighten-5 cursor-pointer csv-dropzone"
              @click="csvState = 'mapping'">
              <VAvatar color="primary" variant="tonal" rounded size="48" class="mb-3">
                <VIcon size="28">tabler-upload</VIcon>
              </VAvatar>
              <div class="text-primary font-weight-medium mb-1">Drag a File or click to browse</div>
              <div class="text-body-2 text-medium-emphasis">File with up to 100 rows works best</div>
            </div>
            <div class="mt-3">
              <a href="#" class="text-primary text-decoration-none text-body-2 d-flex align-center">
                <VIcon size="18" class="mr-1">tabler-cloud-download</VIcon> Download a sample file
              </a>
            </div>
          </VCardText>
        </VCard>

        <!-- CSV Mapping view -->
        <div v-else>
          <VCard class="mb-4" variant="outlined">
            <VCardText class="d-flex align-center justify-space-between py-3">
              <div class="d-flex align-center text-body-1">
                <VIcon color="success" size="20" class="mr-2">tabler-circle-check-filled</VIcon>
                <span class="font-weight-medium mr-4">Upload CSV file Selected</span>
                <VChip size="small" variant="tonal" color="default">Step 1 of 2</VChip>
              </div>
              <VIcon size="20" class="text-medium-emphasis">tabler-chevron-down</VIcon>
            </VCardText>
          </VCard>
          <VCard class="mb-4" variant="outlined">
            <VCardText class="d-flex align-center justify-space-between py-3">
              <div class="d-flex align-center text-body-1">
                <VIcon color="success" size="20" class="mr-2">tabler-circle-check-filled</VIcon>
                <span class="font-weight-medium mr-4">Upload CSV File</span>
                <VChip size="small" variant="tonal" color="default">Step 1 of 2</VChip>
              </div>
              <VIcon size="20" class="text-medium-emphasis">tabler-chevron-down</VIcon>
            </VCardText>
          </VCard>

          <VCard class="mb-6">
            <VCardItem class="pb-2 pt-5">
              <div class="d-flex justify-space-between align-start">
                <div>
                  <VCardTitle class="text-h5 text-primary mb-1">Map Properties</VCardTitle>
                  <div class="d-flex align-center text-body-2 text-primary">
                    <VIcon size="16" class="mr-1">tabler-check</VIcon> Make sure file includes contact name and phone
                    number
                  </div>
                </div>
                <VBtn icon="tabler-trash" variant="text" color="error" />
              </div>
            </VCardItem>

            <VCardText>
              <VRow class="mt-2">
                <VCol cols="12" md="8">
                  <div class="d-flex align-center px-4 mb-2 font-weight-medium text-body-2 text-medium-emphasis">
                    <div style="flex: 1">Contact Field</div>
                    <div style="flex: 1" class="ml-4">CSV Column</div>
                  </div>

                  <div
                    v-for="(field, i) in ['Full name', 'First name', 'Last name', 'Company Name', 'Position', 'Headline']"
                    :key="i" class="d-flex align-center mb-3">
                    <VCard variant="outlined" class="flex-grow-1 px-4 py-2 d-flex align-center text-body-2">
                      <VIcon size="18" class="text-success mr-2">tabler-list-details</VIcon> {{ field }}
                    </VCard>
                    <VCard variant="outlined"
                      class="flex-grow-1 ml-4 px-4 py-2 d-flex align-center justify-space-between text-body-2">
                      <div class="d-flex align-center">
                        <VIcon size="18" class="text-medium-emphasis mr-2">tabler-user</VIcon> {{ field }}
                      </div>
                      <span class="text-medium-emphasis">(35)</span>
                    </VCard>
                  </div>
                </VCol>

                <VCol cols="12" md="4">
                  <VCard variant="outlined" class="h-100 bg-grey-lighten-5">
                    <VCardItem class="pb-2 pt-4">
                      <VCardTitle class="text-subtitle-1">Unmapped Works</VCardTitle>
                    </VCardItem>
                    <VCardText>
                      <VTextField placeholder="Search" prepend-inner-icon="tabler-search" density="compact"
                        variant="outlined" class="mb-4 bg-white" hide-details />
                      <div v-for="item in ['Location', 'Industry', 'Notes']" :key="item" class="mb-2">
                        <VCard variant="outlined"
                          class="px-4 py-2 d-flex align-center justify-space-between bg-white text-body-2">
                          <div class="d-flex align-center">
                            <VIcon size="18" class="text-medium-emphasis mr-2">tabler-list</VIcon> {{ item }} (9)
                          </div>
                          <span class="text-medium-emphasis">(3)</span>
                        </VCard>
                      </div>
                      <div class="text-right mt-3">
                        <a href="#" class="text-primary text-body-2 text-decoration-none">Clear All Matched</a>
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

    <!-- Step 2 Content -->
    <div v-if="currentStep === 2">
      <div class="d-flex mb-6">
        <div class="cursor-pointer border py-2 px-6 rounded-s text-body-1 font-weight-medium"
          :class="currentProfileTab === 'linkedin' ? 'bg-primary-lighten-5 border-primary text-primary' : 'bg-white text-medium-emphasis'"
          @click="currentProfileTab = 'linkedin'" style="transition: all 0.2s ease">
          LinkedIn Profile
        </div>
        <div class="cursor-pointer border border-s-0 py-2 px-6 rounded-e text-body-1 font-weight-medium"
          :class="currentProfileTab === 'email' ? 'bg-primary-lighten-5 border-primary text-primary' : 'bg-white text-medium-emphasis'"
          @click="currentProfileTab = 'email'" style="transition: all 0.2s ease">
          Email Accounts
        </div>
      </div>

      <VCard v-if="currentProfileTab === 'linkedin'" class="mb-6">
        <VCardItem class="pa-4 bg-white">
          <div class="d-flex justify-space-between align-center">
            <div>
              <div class="d-flex align-center text-subtitle-1 font-weight-medium text-high-emphasis">
                <VIcon color="primary" size="20" class="mr-2">tabler-brand-linkedin</VIcon> LinkedIn Profile
              </div>
              <div class="text-body-2 text-medium-emphasis mt-1">Pick which LinkedIn profiles you want to use for this
                campaign.</div>
            </div>
            <VBtn color="primary" size="small">
              <VIcon start size="16">tabler-plus</VIcon> Add Account
            </VBtn>
          </div>
        </VCardItem>

        <VDivider />

        <!-- Table Controls -->
        <VCardText class="d-flex justify-space-between align-center py-3 bg-white">
          <div class="d-flex align-center text-body-2 text-medium-emphasis">
            Show
            <span class="mx-2 d-flex align-center cursor-pointer text-high-emphasis">10 <VIcon size="16" class="ml-1">
                tabler-chevron-down</VIcon></span>
          </div>
          <VTextField placeholder="Search" prepend-inner-icon="tabler-search" density="compact" variant="outlined"
            style="max-width: 250px" hide-details />
        </VCardText>

        <VDivider />

        <VTable class="text-no-wrap bg-white">
          <thead class="bg-grey-lighten-4">
            <tr>
              <th class="text-uppercase text-caption font-weight-bold" style="width: 50px"></th>
              <th class="text-uppercase text-caption font-weight-bold text-high-emphasis">NAME</th>
              <th class="text-uppercase text-caption font-weight-bold text-high-emphasis text-center">HEALTH</th>
              <th class="text-uppercase text-caption font-weight-bold text-high-emphasis text-center">DAILY LIMITS
              </th>
              <th class="text-uppercase text-caption font-weight-bold text-high-emphasis">ACCOUNT TYPE</th>
              <th class="text-uppercase text-caption font-weight-bold text-high-emphasis">STATUS</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>
                <VCheckboxBtn color="primary" />
              </td>
              <td>
                <div class="d-flex align-center py-2">
                  <VAvatar size="40" class="mr-3">
                    <VImg src="https://i.pravatar.cc/150?img=11" />
                  </VAvatar>
                  <div class="d-flex flex-column">
                    <span class="text-subtitle-2 font-weight-medium text-high-emphasis">Edgar Jones</span>
                    <span class="text-caption text-medium-emphasis">1,250 connections</span>
                  </div>
                </div>
              </td>
              <td class="text-center">
                <VProgressCircular :model-value="72" color="warning" size="40" width="3" class="mx-auto">
                  <span class="text-caption font-weight-medium">72</span>
                </VProgressCircular>
              </td>
              <td class="text-center">
                <VChip variant="outlined" size="small" class="bg-white border text-medium-emphasis">Invites: 40 / day
                </VChip>
              </td>
              <td>
                <div class="d-flex align-center text-body-2">
                  <VIcon color="warning" size="20" class="mr-2">tabler-brand-linkedin</VIcon> Premium
                </div>
              </td>
              <td>
                <VChip color="success" size="small" variant="flat">Connected</VChip>
              </td>
            </tr>
          </tbody>
        </VTable>
      </VCard>

      <!-- Email Tab Placeholder -->
      <VCard v-if="currentProfileTab === 'email'" class="mb-6">
        <VCardText class="d-flex justify-center align-center py-10 text-medium-emphasis">
          Email Accounts view coming soon...
        </VCardText>
      </VCard>
    </div>

    <!-- Footer Buttons -->
    <div class="d-flex justify-space-between mt-6">
      <div>
        <VBtn
          v-if="currentStep === 2 || (currentStep === 1 && selectedImportMethod === 'csv' && csvState === 'mapping')"
          variant="text" color="primary" @click="handlePrevious">
          <VIcon start>tabler-arrow-left</VIcon> Previous
        </VBtn>
      </div>
      <div>
        <VBtn v-if="currentStep === 1" color="primary" size="large" @click="handleNext">Next</VBtn>
        <VBtn v-else color="primary" size="large" @click="$emit('submit')">Submit</VBtn>
      </div>
    </div>

    <!-- Lookalike Modal Rendered inside Advance Setup -->
    <LookalikeAudienceModal v-model:isDialogOpen="isLookalikeModalOpen" @listSelected="handleListSelected" />
  </div>
</template>

<style scoped>
.import-card {
  transition: all 0.2s ease-in-out;
}
.import-card:hover {
  border-color: rgba(var(--v-theme-primary), 0.5);
  background-color: rgba(var(--v-theme-primary), 0.02) !important;
}
.border-dashed {
  border-style: dashed !important;
  border-width: 2px !important;
}
</style>
