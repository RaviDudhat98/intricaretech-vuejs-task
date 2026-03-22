<script setup>
import { ref } from 'vue'

const emit = defineEmits(['goBack', 'submit'])

const currentStandardStep = ref(1) // 1 = Leads List, 2 = Target Audience, 3 = Campaign Flow
const campaignName = ref('')
const campaignType = ref('LinkedIn Invitation')
const outreachChannel = ref('LinkedIn Only')
const sendSchedule = ref('Mon - Fri 9:00 AM - 5:00 PM')
const basicLinkedinSearch = ref('')
const useWebhook = ref(false)

</script>

<template>
  <div class="standard-setup-view">

    <VCard class="mb-6 stepper-header-card border-none" elevation="0">
      <VCardText class="d-flex flex-column pa-4 pb-0">
        <div class="d-flex align-center justify-space-between px-6 py-4 border rounded shadow-sm bg-white">
          <!-- Step 1 -->
          <div
            :class="['d-flex align-center font-weight-semibold cursor-pointer', currentStandardStep === 1 ? 'text-textPrimary' : 'text-disabled']"
            @click="currentStandardStep = 1">
            <VAvatar :color="currentStandardStep === 1 ? 'newPrimary' : 'activebg'"
              :variant="currentStandardStep === 1 ? 'elevated' : 'flat'" rounded size="38" class="mr-3">
              <VIcon size="22" :color="currentStandardStep === 1 ? 'white' : 'newPrimary'">tabler-list-details</VIcon>
            </VAvatar>
            <span class="text-subtitle-1">Leads List</span>
          </div>
          <VIcon size="20" class="text-disabled">tabler-chevron-right</VIcon>

          <!-- Step 2 -->
          <div
            :class="['d-flex align-center font-weight-semibold cursor-pointer', currentStandardStep === 2 ? 'text-textPrimary' : 'text-disabled']"
            @click="currentStandardStep = 2">
            <VAvatar :color="currentStandardStep === 2 ? 'newPrimary' : 'activebg'"
              :variant="currentStandardStep === 2 ? 'elevated' : 'flat'" rounded size="38" class="mr-3">
              <VIcon size="22" :color="currentStandardStep === 2 ? 'white' : 'newPrimary'">tabler-send</VIcon>
            </VAvatar>
            <span class="text-subtitle-1">Target Audience</span>
          </div>
          <VIcon size="20" class="text-disabled">tabler-chevron-right</VIcon>

          <!-- Step 3 -->
          <div
            :class="['d-flex align-center font-weight-semibold cursor-pointer', currentStandardStep === 3 ? 'text-textPrimary' : 'text-disabled']"
            @click="currentStandardStep = 3">
            <VAvatar :color="currentStandardStep === 3 ? 'newPrimary' : 'activebg'"
              :variant="currentStandardStep === 3 ? 'elevated' : 'flat'" rounded size="38" class="mr-3">
              <VIcon size="22" :color="currentStandardStep === 3 ? 'white' : 'newPrimary'">tabler-speakerphone</VIcon>
            </VAvatar>
            <span class="text-subtitle-1">Campaign</span>
          </div>
        </div>


        <!-- Standard Step 1: Leads List -->
        <div v-if="currentStandardStep === 1" class="px-10 py-5">
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
                  :class="outreachChannel === 'Email Only' ? 'bg-newPrimary-lighten-5 text-newPrimary' : 'bg-white text-medium-emphasis'"
                  @click="outreachChannel = 'Email Only'">Email Only</div>
                <div
                  class="cursor-pointer py-2 text-center text-body-2 flex-grow-1 font-weight-medium border-s border-e"
                  :class="outreachChannel === 'LinkedIn Only' ? 'bg-newPrimary-lighten-5 text-newPrimary' : 'bg-white text-medium-emphasis'"
                  @click="outreachChannel = 'LinkedIn Only'">LinkedIn Only</div>
                <div class="cursor-pointer py-2 text-center text-body-2 flex-grow-1 font-weight-medium rounded-e"
                  :class="outreachChannel === 'LinkedIn + Email' ? 'bg-newPrimary-lighten-5 text-newPrimary' : 'bg-white text-medium-emphasis'"
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
                <VBtn variant="outlined" color="newPrimary" class="text-none bg-white">
                  <VIcon start>tabler-plus</VIcon> Add new time
                </VBtn>
              </div>
              <div class="text-caption text-medium-emphasis">Messages are sent only within your selected hours.</div>
            </VCol>
          </VRow>

          <div class="d-flex justify-end mt-12">
            <VBtn class="bg-bgGradient" @click="currentStandardStep = 2">Continue</VBtn>
          </div>
        </div>

        <!-- Standard Step 2: Target Audience -->
        <div v-if="currentStandardStep === 2" class="py-5">
          <VRow>
            <VCol cols="12" md="6">
              <VCard variant="outlined" class="h-100 pa-6 bg-white border-dashed">
                <div class="text-subtitle-1 font-weight-medium mb-3">Basic LinkedIn Search</div>
                <VTextField v-model="basicLinkedinSearch"
                  placeholder="https://www.linkedin.com/search/results/people/..." variant="outlined" density="compact"
                  hide-details class="mb-3" />
                <div class="text-caption text-medium-emphasis">Filter profiles in the <a href="#"
                    class="text-newPrimary text-decoration-none">LinkedIn search</a> and paste the URL above</div>
              </VCard>
            </VCol>
            <VCol cols="12" md="3">
              <VCard variant="outlined" class="h-100 pa-6 d-flex flex-column bg-white border-dashed">
                <div class="text-subtitle-1 font-weight-medium mb-3">Upload CSV File</div>
                <div
                  class="flex-grow-1 border-dashed border-newPrimary! rounded d-flex flex-column align-center justify-center bg-newPrimary-lighten-5 cursor-pointer"
                  style="min-height: 100px;">
                  <VIcon color="newPrimary" size="24" class="mb-1">tabler-upload</VIcon>
                  <div class="text-newPrimary text-body-2 font-weight-medium">Drop file here</div>
                </div>
              </VCard>
            </VCol>
            <VCol cols="12" md="3">
              <VCard variant="outlined" class="h-100 pa-6 bg-white border-dashed">
                <div class="text-subtitle-1 font-weight-medium mb-3">Advanced options</div>
                <VCheckbox v-model="useWebhook" label="Use Webhook" color="newPrimary" density="compact" hide-details
                  class="mb-2" />
                <div class="text-caption text-medium-emphasis">Use a webhook to send leads automatically.</div>
              </VCard>
            </VCol>
          </VRow>

          <div class="d-flex justify-space-between align-center mt-12">
            <div class="cursor-pointer text-newPrimary text-body-1 font-weight-medium" @click="$emit('goBack')">Back
            </div>
            <div class="d-flex gap-4">
              <VBtn variant="tonal" color="secondary" class="bg-grey-lighten-3">Save as Draft</VBtn>
              <VBtn class="bg-bgGradient" @click="currentStandardStep = 3">Continue</VBtn>
            </div>
          </div>
        </div>

        <!-- Standard Step 3: Sequence -->
        <div v-if="currentStandardStep === 3" class="sequence-timeline px-4">
          <div class="timeline-container py-2 border-s-2 border-newPrimary ml-2 mb-4 position-relative">

            <!-- Stop 1: Campaign Start -->
            <div class="timeline-dot position-absolute bg-white border-2 border-newPrimary rounded-circle"
              style="left: -9px; top: 0; width: 16px; height: 16px;"></div>
            <VCard variant="outlined" class="my-6 ml-6 border" elevation="0">
              <div class="d-flex align-center mx-4 my-3">
                <VIcon color="newPrimary" size="24" class="mr-3">tabler-rocket</VIcon>
                <span class="text-h6 font-weight-bold text-textPrimary">Campaign Start</span>
              </div>
              <div class="px-4 py-1 bg-activebg rounded-b text-body-2 text-textPrimary">
                When a lead enters your target audience
              </div>
            </VCard>

            <!-- Stop 2: Connection Request -->
            <div class="timeline-dot position-absolute bg-white border-2 border-newPrimary rounded-circle"
              style="left: -9px; top: 125px; width: 16px; height: 16px;"></div>
            <VCard variant="outlined" class="mb-8 ml-6 border">
              <VCardText class="d-flex align-center justify-space-between py-4 ">
                <div class="d-flex align-center text-h6 font-weight-bold text-textPrimary">
                  <VIcon size="20" color="newPrimary" class="mr-3">tabler-brand-linkedin</VIcon>
                  Send LinkedIn Connection Request
                </div>
                <div class="d-flex gap-3">
                  <VIcon size="20" class="text-medium-emphasis cursor-pointer">tabler-pencil</VIcon>
                  <VIcon size="20" color="error" class="cursor-pointer">tabler-trash</VIcon>
                </div>
              </VCardText>
              <VCardText class="pa-6">
                <div class="text-body-1 mb-6 pa-5 rounded border border-dashed text-textPrimary bg-bgLight"
                  style="min-height: 100px;">
                  Hi {{ '{first_name}' }}..
                </div>
                <div class="d-flex gap-4">
                  <VBtn class="bg-bgGradient text-white px-8" elevation="0">Edit Message</VBtn>
                  <VBtn variant="outlined" color="newPrimary" class="px-8 border-newPrimary">
                    <VIcon start>tabler-wand</VIcon> Make with AI
                  </VBtn>
                </div>
              </VCardText>
            </VCard>

            <!-- Stop 3: Follow-up message -->
            <div class="timeline-dot position-absolute bg-white border-2 border-newPrimary rounded-circle"
              style="left: -9px; top: 415px; width: 16px; height: 16px;"></div>
            <VCard variant="outlined" class="mb-8 ml-6 border">
              <VCardText class="d-flex align-center justify-space-between py-4 ">
                <div class="d-flex align-center text-h6 font-weight-bold text-textPrimary">
                  <VIcon size="20" color="newPrimary" class="mr-3">tabler-arrow-forward-up</VIcon>
                  Set Follow-up message
                </div>
                <div class="d-flex gap-3">
                  <VIcon size="20" class="text-medium-emphasis cursor-pointer">tabler-pencil</VIcon>
                  <VIcon size="20" color="error" class="cursor-pointer">tabler-trash</VIcon>
                </div>
              </VCardText>
              <VCardText class="pa-6">
                <div class="text-body-1 mb-6 pa-5 rounded border border-dashed text-textPrimary bg-bgLight"
                  style="min-height: 100px;">
                  Hi {{ '{first_name}' }}..
                </div>
                <div class="d-flex gap-4 mb-6">
                  <VBtn class="bg-bgGradient text-white px-8" elevation="0">Edit Message</VBtn>
                  <VBtn variant="outlined" color="newPrimary" class="px-8 border-newPrimary">
                    <VIcon start>tabler-wand</VIcon> Make with AI
                  </VBtn>
                </div>

                <!-- Wait section -->
                <div
                  class="pa-5 rounded border border-dashed border-newPrimary bg-bgLight d-flex align-center text-body-1 text-textSecondary font-weight-medium">
                  Once accepted wait
                  <span class="mx-3 d-flex align-center bg-white border rounded px-3 py-1">
                    3 <span class="ml-2 text-disabled font-weight-regular text-body-2">Minutes</span>
                  </span>
                  <span class="mr-3 d-flex align-center bg-white border rounded px-3 py-1">
                    3 <span class="ml-2 text-disabled font-weight-regular text-body-2">Hour</span>
                  </span>
                  <span class="d-flex align-center bg-white border rounded px-3 py-1">
                    3 <span class="ml-2 text-disabled font-weight-regular text-body-2">days</span>
                  </span>
                </div>
              </VCardText>
            </VCard>

            <!-- Stop 4: Add new follow-up -->
            <div class="timeline-dot position-absolute bg-white border-2 border-newPrimary rounded-circle"
              style="left: -9px; top: 825px; width: 16px; height: 16px;"></div>
            <VCard variant="outlined" class="mb-8 ml-6 border-dashed border-newPrimary bg-bgLight cursor-pointer"
              style="border-width: 2px !important;">
              <VCardText class="d-flex py-5 text-newPrimary align-center font-weight-bold text-h6">
                <VIcon size="24" class="mr-3 font-weight-bold">tabler-circle-plus</VIcon> Add new follow-up
              </VCardText>
            </VCard>

            <!-- Stop 5: End of Campaign -->
            <div class="timeline-dot position-absolute bg-white border-2 border-newPrimary rounded-circle"
              style="left: -9px; bottom: 20px; width: 16px; height: 16px;"></div>
            <VCard variant="flat" class="ml-6 bg-activebg">
              <VCardText class="d-flex align-center py-4 text-newPrimary font-weight-bold text-h6">
                <VIcon size="24" class="mr-3">tabler-minus</VIcon> End of Campaign
              </VCardText>
            </VCard>

          </div>

          <div class="d-flex justify-space-between align-center mt-12 mb-6">
            <div class="cursor-pointer text-newPrimary text-h6 font-weight-medium" @click="currentStandardStep = 2">
              Back
            </div>
            <div class="d-flex gap-4">
              <VBtn variant="tonal" color="secondary" class="bg-grey-lighten-3 px-8 text-none">Save as
                Draft</VBtn>
              <VBtn class="bg-bgGradient text-white px-8 text-none" @click="$emit('submit')">Launch
                Campaign</VBtn>
            </div>
          </div>
        </div>
      </VCardText>
    </VCard>
  </div>
</template>

<style scoped>
.bg-bgGradient {
  background: linear-gradient(239.27deg, #8BA6FF -27.06%, #3762EE 83.4%) !important;
}

.gap-2 {
  gap: 8px;
}

.gap-4 {
  gap: 16px;
}

.border-dashed {
  border-style: dashed !important;
  border-width: 2px !important;
}
</style>
