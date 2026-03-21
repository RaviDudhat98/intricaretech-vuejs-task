<script setup>
import { ref } from 'vue'

const currentView = ref('empty')
const isWorkflowModalOpen = ref(false)
const selectedWorkflow = ref('advanced')

const currentStep = ref(1) // 1 = Define Target Audience, 2 = Sender Profiles

const selectedImportMethod = ref('linkedin')

// CSV Mapping logic
const csvState = ref('upload') // 'upload' | 'mapping'

// Lookalike Modal logic
const isLookalikeModalOpen = ref(false)
const hasLeads = ref(false)
const selectedLookalikeList = ref(null)

// Sender Profiles logic
const currentProfileTab = ref('linkedin')

// Standard Flow State
const currentStandardStep = ref(1) // 1 = Leads List, 2 = Target Audience, 3 = Campaign Flow
const campaignName = ref('')
const campaignType = ref('LinkedIn Invitation')
const outreachChannel = ref('LinkedIn Only')
const sendSchedule = ref('Mon - Fri 9:00 AM - 5:00 PM')
const basicLinkedinSearch = ref('')
const useWebhook = ref(false)

const openModal = () => {
  isWorkflowModalOpen.value = true
}

const goToAdvanceCampaign = () => {
  isWorkflowModalOpen.value = false
  if (selectedWorkflow.value === 'advanced') {
    currentView.value = 'advance'
    currentStep.value = 1
  } else if (selectedWorkflow.value === 'standard') {
    currentView.value = 'standard'
    currentStandardStep.value = 1
  }
}

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
</script>

<template>
  <div class="campaign-page">
    <!-- View 1: Empty State -->
    <div v-if="currentView === 'empty'" class="empty-state-view">
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
          @click="openModal">
          New Campaign
        </VBtn>
      </div>
    </div>

    <!-- View 2: Advance Campaign Setup -->
    <div v-if="currentView === 'advance'" class="advance-setup-view">
      <div class="d-flex align-center mb-6">
        <h2 class="text-h6 font-weight-regular text-primary cursor-pointer mb-0" @click="currentView = 'empty'">Campaign
        </h2>
        <VIcon size="20" class="mx-2 text-medium-emphasis">tabler-chevron-right</VIcon>
        <h2 class="text-h6 font-weight-regular mb-0 text-high-emphasis">Advance Campaign</h2>
      </div>

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
          <VBtn v-else color="primary" size="large">Submit</VBtn>
        </div>
      </div>
    </div>

    <!-- View 3: Standard Campaign Setup -->
    <div v-if="currentView === 'standard'" class="standard-setup-view">
      <div class="d-flex align-center mb-6">
        <h2 class="text-h6 font-weight-regular text-primary cursor-pointer mb-0" @click="currentView = 'empty'">Campaign
        </h2>
        <VIcon size="20" class="mx-2 text-medium-emphasis">tabler-chevron-right</VIcon>
        <h2 class="text-h6 font-weight-regular mb-0 text-high-emphasis">Advance Campaign</h2>
      </div>

      <!-- Stepper area for Standard Flow -->
      <VCard class="mb-6">
        <VCardText class="d-flex align-center pa-4">
          <div
            :class="['d-flex align-center font-weight-medium cursor-pointer', currentStandardStep >= 1 ? 'text-primary' : 'text-disabled']"
            @click="currentStandardStep = 1">
            <VAvatar :color="currentStandardStep >= 1 ? 'primary' : 'secondary'"
              :variant="currentStandardStep >= 1 ? 'elevated' : 'tonal'" rounded size="32" class="mr-2"
              :class="{ 'opacity-50': currentStandardStep < 1 }">
              <VIcon size="20">tabler-list</VIcon>
            </VAvatar>
            Leads List
          </div>
          <VIcon size="20" class="mx-4 text-disabled">tabler-chevron-right</VIcon>

          <div
            :class="['d-flex align-center font-weight-medium cursor-pointer', currentStandardStep >= 2 ? 'text-primary' : 'text-disabled']"
            @click="currentStandardStep = 2">
            <VAvatar :color="currentStandardStep >= 2 ? 'primary' : 'secondary'"
              :variant="currentStandardStep >= 2 ? 'elevated' : 'tonal'" rounded size="32" class="mr-2"
              :class="{ 'opacity-50': currentStandardStep < 2 }">
              <VIcon size="20">tabler-send</VIcon>
            </VAvatar>
            Target Audience
          </div>
          <VIcon size="20" class="mx-4 text-disabled">tabler-chevron-right</VIcon>

          <div
            :class="['d-flex align-center font-weight-medium', currentStandardStep === 3 ? 'text-primary' : 'text-disabled']">
            <VAvatar :color="currentStandardStep === 3 ? 'primary' : 'secondary'"
              :variant="currentStandardStep === 3 ? 'elevated' : 'tonal'" rounded size="32" class="mr-2"
              :class="{ 'opacity-50': currentStandardStep !== 3 }">
              <VIcon size="20">tabler-speakerphone</VIcon>
            </VAvatar>
            Campaign
          </div>
        </VCardText>
      </VCard>

      <!-- Standard Step 1: Leads List -->
      <div v-if="currentStandardStep === 1">
        <VRow>
          <VCol cols="12" md="6">
            <div class="text-subtitle-2 font-weight-medium mb-1">Campaign Name</div>
            <VTextField v-model="campaignName" placeholder="e.g. Founder Outreach - India" variant="outlined"
              density="compact" hint="Give your workflow a clear name so you can find it later." persistent-hint />
          </VCol>
          <VCol cols="12" md="6">
            <div class="text-subtitle-2 font-weight-medium mb-1">Campaign Type</div>
            <VSelect v-model="campaignType" :items="['LinkedIn Invitation', 'Email Followup']" variant="outlined"
              density="compact" hint="Choose how this workflow will start." persistent-hint />
          </VCol>
          <VCol cols="12" md="6">
            <div class="text-subtitle-2 font-weight-medium mb-1 mt-4">Outreach Channel</div>
            <div class="d-flex border rounded mb-1">
              <div class="cursor-pointer py-2 text-center text-body-2 flex-grow-1 font-weight-medium rounded-s"
                :class="outreachChannel === 'Email Only' ? 'bg-primary-lighten-5 text-primary' : 'bg-white text-medium-emphasis'"
                @click="outreachChannel = 'Email Only'">Email Only</div>
              <div class="cursor-pointer py-2 text-center text-body-2 flex-grow-1 font-weight-medium border-s border-e"
                :class="outreachChannel === 'LinkedIn Only' ? 'bg-primary-lighten-5 text-primary' : 'bg-white text-medium-emphasis'"
                @click="outreachChannel = 'LinkedIn Only'">LinkedIn Only</div>
              <div class="cursor-pointer py-2 text-center text-body-2 flex-grow-1 font-weight-medium rounded-e"
                :class="outreachChannel === 'LinkedIn + Email' ? 'bg-primary-lighten-5 text-primary' : 'bg-white text-medium-emphasis'"
                @click="outreachChannel = 'LinkedIn + Email'">LinkedIn + Email</div>
            </div>
            <div class="text-caption text-medium-emphasis">Select how you want to reach your leads in this workflow.
            </div>
          </VCol>
          <VCol cols="12" md="6">
            <div class="text-subtitle-2 font-weight-medium mb-1 mt-4">Sending Schedule</div>
            <div class="d-flex gap-4 mb-1">
              <VTextField v-model="sendSchedule" variant="outlined" density="compact" append-inner-icon="tabler-clock"
                hide-details class="flex-grow-1 bg-white" />
              <VBtn variant="outlined" color="primary" class="text-none bg-white">
                <VIcon start>tabler-plus</VIcon> Add new time
              </VBtn>
            </div>
            <div class="text-caption text-medium-emphasis">Messages are sent only within your selected hours.</div>
          </VCol>
        </VRow>

        <div class="d-flex justify-end mt-12">
          <VBtn color="primary" @click="currentStandardStep = 2">Continue</VBtn>
        </div>
      </div>

      <!-- Standard Step 2: Target Audience -->
      <div v-if="currentStandardStep === 2">
        <VRow>
          <VCol cols="12" md="6">
            <VCard variant="outlined" class="h-100 pa-6 bg-white border-dashed">
              <div class="text-subtitle-1 font-weight-medium mb-3">Basic LinkedIn Search</div>
              <VTextField v-model="basicLinkedinSearch" placeholder="https://www.linkedin.com/search/results/people/..."
                variant="outlined" density="compact" hide-details class="mb-3" />
              <div class="text-caption text-medium-emphasis">Filter profiles in the <a href="#"
                  class="text-primary text-decoration-none">LinkedIn search</a> and paste the URL above</div>
            </VCard>
          </VCol>
          <VCol cols="12" md="3">
            <VCard variant="outlined" class="h-100 pa-6 d-flex flex-column bg-white border-dashed">
              <div class="text-subtitle-1 font-weight-medium mb-3">Upload CSV File</div>
              <div
                class="flex-grow-1 border-dashed border-primary rounded d-flex flex-column align-center justify-center bg-primary-lighten-5 cursor-pointer"
                style="min-height: 100px;">
                <VIcon color="primary" size="24" class="mb-1">tabler-upload</VIcon>
                <div class="text-primary text-body-2 font-weight-medium">Drop file here</div>
              </div>
            </VCard>
          </VCol>
          <VCol cols="12" md="3">
            <VCard variant="outlined" class="h-100 pa-6 bg-white border-dashed">
              <div class="text-subtitle-1 font-weight-medium mb-3">Advanced options</div>
              <VCheckbox v-model="useWebhook" label="Use Webhook" color="primary" density="compact" hide-details
                class="mb-2" />
              <div class="text-caption text-medium-emphasis">Use a webhook to send leads automatically.</div>
            </VCard>
          </VCol>
        </VRow>

        <div class="d-flex justify-space-between align-center mt-12">
          <div class="cursor-pointer text-primary text-body-1 font-weight-medium" @click="currentStandardStep = 1">Back
          </div>
          <div class="d-flex gap-4">
            <VBtn variant="tonal" color="secondary" class="bg-grey-lighten-3">Save as Draft</VBtn>
            <VBtn color="primary" @click="currentStandardStep = 3">Continue</VBtn>
          </div>
        </div>
      </div>

      <!-- Standard Step 3: Sequence -->
      <div v-if="currentStandardStep === 3" class="sequence-timeline">
        <div class="timeline-container px-6 py-2 border-s-2 border-primary ml-4 mb-4 position-relative">

          <!-- Stop 1 -->
          <div class="timeline-dot position-absolute bg-white border border-primary rounded-circle"
            style="left: -7px; top: 0; width: 12px; height: 12px;"></div>
          <VCard variant="outlined" class="bg-grey-lighten-5 mb-6 mt-n2 ml-4">
            <VCardText class="d-flex align-center py-3">
              <VIcon color="medium-emphasis" size="20" class="mr-2">tabler-arrow-ramp-right-2</VIcon>
              <span class="text-body-2 font-weight-medium text-high-emphasis">Campaign Start</span>
            </VCardText>
            <VDivider />
            <VCardText class="py-2 bg-white">
              <span class="text-caption text-medium-emphasis">When a lead enters your target audience</span>
            </VCardText>
          </VCard>

          <!-- Stop 2 -->
          <div class="timeline-dot position-absolute bg-white border border-primary rounded-circle"
            style="left: -7px; top: 110px; width: 12px; height: 12px;"></div>
          <VCard variant="outlined" class="mb-6 ml-4">
            <VCardText class="d-flex align-center justify-space-between py-3 border-b">
              <div class="d-flex align-center text-body-2 font-weight-medium text-high-emphasis">
                <VAvatar color="primary" variant="tonal" rounded size="24" class="mr-2 px-1">
                  <VIcon size="14">tabler-brand-linkedin</VIcon>
                </VAvatar>
                Send LinkedIn Connection Request
              </div>
              <div class="d-flex gap-2">
                <VIcon size="18" class="text-medium-emphasis cursor-pointer">tabler-pencil</VIcon>
                <VIcon size="18" color="error" class="cursor-pointer">tabler-trash</VIcon>
              </div>
            </VCardText>
            <VCardText class="pt-4">
              <div class="text-body-2 mb-4 bg-white rounded border pa-3 text-medium-emphasis">
                Hi {{ '{first_name}' }}..
              </div>
              <div class="d-flex gap-4">
                <VBtn color="primary" size="small">Edit Message</VBtn>
                <VBtn variant="outlined" color="primary" size="small">
                  <VIcon start>tabler-wand</VIcon> Make with AI
                </VBtn>
              </div>
            </VCardText>
          </VCard>

          <!-- Stop 3 -->
          <div class="timeline-dot position-absolute bg-white border border-primary rounded-circle"
            style="left: -7px; top: 320px; width: 12px; height: 12px;"></div>
          <VCard variant="outlined" class="mb-6 ml-4">
            <VCardText class="d-flex align-center justify-space-between py-3 border-b">
              <div class="d-flex align-center text-body-2 font-weight-medium text-high-emphasis">
                <VAvatar color="info" variant="tonal" rounded size="24" class="mr-2">
                  <VIcon size="14">tabler-arrow-forward-up</VIcon>
                </VAvatar>
                Set Follow-up message
              </div>
              <div class="d-flex gap-2">
                <VIcon size="18" class="text-medium-emphasis cursor-pointer">tabler-pencil</VIcon>
                <VIcon size="18" color="error" class="cursor-pointer">tabler-trash</VIcon>
              </div>
            </VCardText>
            <VCardText class="pt-4">
              <div class="text-body-2 mb-4 bg-white rounded border pa-3 text-medium-emphasis">
                Hi {{ '{first_name}' }}..
              </div>
              <div class="d-flex gap-4 mb-4">
                <VBtn color="primary" size="small">Edit Message</VBtn>
                <VBtn variant="outlined" color="primary" size="small">
                  <VIcon start>tabler-wand</VIcon> Make with AI
                </VBtn>
              </div>
            </VCardText>
            <VDivider class="border-dashed" />
            <VCardText class="py-3 bg-grey-lighten-5 d-flex align-center text-body-2 text-medium-emphasis">
              Once accepted wait
              <span class="d-inline-flex bg-white border rounded px-2 py-1 mx-2">3 <span
                  class="ml-1 text-disabled">Minutes</span></span>
              <span class="d-inline-flex bg-white border rounded px-2 py-1 mx-2">3 <span
                  class="ml-1 text-disabled">Hour</span></span>
              <span class="d-inline-flex bg-white border rounded px-2 py-1 mx-2">3 <span
                  class="ml-1 text-disabled">days</span></span>
            </VCardText>
          </VCard>

          <!-- Stop 4 (Add new) -->
          <div class="timeline-dot position-absolute bg-white border border-primary rounded-circle"
            style="left: -7px; top: 560px; width: 12px; height: 12px;"></div>
          <VCard variant="outlined" class="mb-6 ml-4 bg-grey-lighten-5 border-dashed cursor-pointer"
            style="border-width: 2px !important;">
            <VCardText class="d-flex align-center py-4 text-primary justify-center font-weight-medium">
              <VIcon size="20" class="mr-2">tabler-plus</VIcon> Add new follow-up
            </VCardText>
          </VCard>

          <!-- Stop 5 (End) -->
          <div class="timeline-dot position-absolute bg-white border border-primary rounded-circle"
            style="left: -7px; bottom: 18px; width: 12px; height: 12px;"></div>
          <VCard variant="flat" class="bg-primary-lighten-5 ml-4">
            <VCardText class="d-flex align-center py-3 text-primary font-weight-medium">
              <VIcon size="20" class="mr-2">tabler-minus</VIcon> End of Campaign
            </VCardText>
          </VCard>

        </div>

        <div class="d-flex justify-space-between align-center mt-8">
          <div class="cursor-pointer text-primary text-body-1 font-weight-medium" @click="currentStandardStep = 2">Back
          </div>
          <div class="d-flex gap-4">
            <VBtn variant="tonal" color="secondary" class="bg-grey-lighten-3">Save as Draft</VBtn>
            <VBtn color="primary">Launch Campaign</VBtn>
          </div>
        </div>
      </div>
    </div>

    <!-- Workflow Mode Modal -->
    <VDialog v-model="isWorkflowModalOpen" max-width="600">
      <VCard>
        <VCardItem class="pb-3 border-b">
          <div class="d-flex justify-space-between align-start px-1 pt-2">
            <div>
              <VCardTitle class="text-h5">Select Workflow Mode</VCardTitle>
              <VCardSubtitle>Choose how you want your campaign to behave</VCardSubtitle>
            </div>
            <VBtn icon="tabler-x" variant="text" color="medium-emphasis" @click="isWorkflowModalOpen = false" />
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
          <VBtn color="secondary" variant="tonal" @click="isWorkflowModalOpen = false">Close</VBtn>
          <VBtn color="primary" variant="elevated" @click="goToAdvanceCampaign">Next</VBtn>
        </VCardActions>
      </VCard>
    </VDialog>

    <!-- Lookalike Modal -->
    <VDialog v-model="isLookalikeModalOpen" max-width="500">
      <VCard>
        <VCardItem class="pb-3 border-b">
          <div class="d-flex justify-space-between align-start pt-2 px-1">
            <div>
              <VCardTitle class="text-h5">Lookalikes</VCardTitle>
              <VCardSubtitle>Select a lookalike list for this campaign</VCardSubtitle>
            </div>
            <VBtn icon="tabler-x" variant="text" color="medium-emphasis" @click="isLookalikeModalOpen = false" />
          </div>
        </VCardItem>

        <!-- Empty State -->
        <VCardText v-if="!hasLeads" class="d-flex flex-column justify-center align-center py-10">
          <h4 class="text-h6 text-primary mb-1 mt-6">You don't have any leads</h4>
          <p class="text-body-2 text-medium-emphasis mb-6">Create a lead list to start running campaigns</p>
          <VBtn color="primary" @click="hasLeads = true" class="mb-4">Create a List</VBtn>
        </VCardText>

        <!-- Populated State -->
        <div v-else>
          <VCardText class="pt-6 pb-2">
            <!-- List 1 -->
            <VCard variant="outlined" class="mb-3 cursor-pointer workflow-card"
              :class="{ 'border-primary bg-primary-lighten-5': selectedLookalikeList === 'founder' }"
              @click="selectedLookalikeList = 'founder'">
              <VCardText class="d-flex align-center justify-space-between py-3 px-4">
                <div class="d-flex align-center">
                  <VIcon size="20" class="mr-3 text-medium-emphasis">tabler-list</VIcon>
                  <span class="text-body-1 font-weight-medium mr-2">Founder</span>
                  <span class="text-body-2 text-medium-emphasis">(1000+ Users in the List)</span>
                </div>
                <VCheckboxBtn :model-value="selectedLookalikeList === 'founder'" color="primary" />
              </VCardText>
            </VCard>

            <!-- List 2 -->
            <VCard variant="outlined" class="mb-2 cursor-pointer workflow-card"
              :class="{ 'border-primary bg-primary-lighten-5': selectedLookalikeList === 'tech' }"
              @click="selectedLookalikeList = 'tech'">
              <VCardText class="d-flex align-center justify-space-between py-3 px-4">
                <div class="d-flex align-center">
                  <VIcon size="20" class="mr-3 text-medium-emphasis">tabler-list</VIcon>
                  <span class="text-body-1 font-weight-medium mr-2">Tech Profiles</span>
                  <span class="text-body-2 text-medium-emphasis">(1000+ Users in the List)</span>
                </div>
                <VCheckboxBtn :model-value="selectedLookalikeList === 'tech'" color="primary" />
              </VCardText>
            </VCard>

            <div class="text-right mt-3 mb-2">
              <a href="#" class="text-primary text-body-2 text-decoration-none">Add New</a>
            </div>
          </VCardText>

          <VCardActions class="px-6 py-4 border-t d-flex justify-end gap-3">
            <VBtn color="secondary" variant="tonal" @click="isLookalikeModalOpen = false">Cancel</VBtn>
            <VBtn color="primary" variant="elevated" @click="isLookalikeModalOpen = false">Select List</VBtn>
          </VCardActions>
        </div>
      </VCard>
    </VDialog>
  </div>
</template>

<style scoped>
.v-input :deep(.v-field__input) {
  min-height: 38px !important;
}

.gap-3 {
  gap: 12px;
}

.gap-4 {
  gap: 16px;
}

.workflow-card {
  transition: all 0.2s ease-in-out;
}

.workflow-card:hover {
  border-color: rgba(var(--v-theme-primary), 0.5);
}

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
