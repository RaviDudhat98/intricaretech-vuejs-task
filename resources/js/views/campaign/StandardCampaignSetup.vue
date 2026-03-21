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
        <div class="cursor-pointer text-primary text-body-1 font-weight-medium" @click="$emit('goBack')">Back
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
          <VBtn color="primary" @click="$emit('submit')">Launch Campaign</VBtn>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.gap-2 { gap: 8px; }
.gap-4 { gap: 16px; }
.border-dashed {
  border-style: dashed !important;
  border-width: 2px !important;
}
</style>
