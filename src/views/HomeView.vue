<template>
  <v-container class="align-center">
    <AppImagesView :images="['/logos/abacus-summer-party-2024-banner.png']">
      <template #activator="activatorProps">
        <v-img
          v-bind="activatorProps"
          class="rounded-lg"
          src="/logos/abacus-summer-party-2024-banner.png"
          max-height="400"
          cover
        >
        </v-img>
      </template>
    </AppImagesView>
  </v-container>
  <v-container class="align-center">
    <v-row>
      <v-col v-for="link in sortedLinks" :key="link.id" cols="12" sm="6" lg="3">
        <v-card
          variant="tonal"
          color="primary"
          :to="link.to"
          height="100%"
          :density="$vuetify.display.xs ? 'compact' : 'default'"
          @click="
            link.action && Object.keys(link).includes('action')
              ? link.action()
              : undefined
          "
        >
          <template #title>
            <v-card-title class="text-h6 text-sm-h5">
              {{ link.title }}
              <AppLiveLabel v-if="link.id === 'parking'" />
            </v-card-title>
          </template>
          <template #subtitle>
            <v-card-subtitle class="text-wrap">
              {{ link.description }}
            </v-card-subtitle>
          </template>
          <template #append>
            <v-avatar rounded="0" size="40">
              <v-icon size="40" color="primary">{{ link.icon }}</v-icon>
            </v-avatar>
          </template>
        </v-card>
      </v-col>
    </v-row>
    <AppImagesView v-model="showGallery" :images="['/images/plan.png']" />
  </v-container>
</template>

<script setup lang="ts">
import { useI18n } from 'vue-i18n';
import AppLiveLabel from '../components/app/AppLiveLabel.vue';
import AppImagesView from '@/components/app/AppImagesView.vue';
import { useAuthStore } from '@/stores/auth';

const { t } = useI18n();

const { isCurrentUserAdminOrHelper } = useAuthStore();

const links = computed(() => [
  {
    id: 'dashboard',
    title: 'Dashboard',
    icon: 'mdi-view-dashboard',
    show: () => isCurrentUserAdminOrHelper,
    to: { name: 'dashboard' }
  },
  {
    id: 'program',
    title: t('home.links.program.title'),
    description: t('home.links.program.description'),
    icon: 'mdi-clipboard-text',
    show: () => true,
    to: { name: 'program' }
  },
  {
    id: 'parking',
    title: t('home.links.parking.title'),
    description: t('home.links.parking.description'),
    icon: 'mdi-car',
    show: () => true,
    to: { name: 'parking' }
  },
  {
    id: 'shuttle-schedule',
    title: t('home.links.shuttle_schedule.title'),
    description: t('home.links.shuttle_schedule.description'),
    icon: 'mdi-bus',
    show: () => true,
    to: { name: 'shuttle-schedule' }
  },
  {
    id: 'plan',
    title: t('labels.plan'),
    description: undefined,
    icon: 'mdi-map',
    show: () => true,
    action: () => {
      showGallery.value = true;
    }
  }
]);

const sortedLinks = computed(() => links.value.filter((l) => l.show()));

const showGallery = ref(false);
</script>
