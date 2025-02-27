<template>
  <NeoSidebar
    fullheight
    fullwidth
    overlay
    position="fixed"
    :open="open"
    :can-cancel="['escape']"
    :on-cancel="onClose"
    class="top is-absolute background-color">
    <div class="is-flex is-flex-direction-column is-fullheight">
      <div class="is-flex-grow-1">
        <div class="is-flex border-bottom">
          <p class="card-header-title has-text-weight-bold">
            {{ $t('general.filters') }}
          </p>
          <a class="card-header-icon">
            <NeoIcon icon="x" @click="onClose" />
          </a>
        </div>
        <EventTypeFilter
          v-if="isCollectionActivityTab"
          data-model="store"
          expanded />
        <StatusFilter v-else data-model="store" expanded />
        <PriceFilter
          v-if="!isCollectionActivityTab"
          data-model="store"
          expanded />
        <PopularCollections v-if="isExploreItems" data-model="store" expanded />
        <AdvancedFilter v-if="!isCollectionActivityTab" data-model="store" />
      </div>

      <div
        class="buttons-container px-4 py-3 border-top theme-background-color">
        <NeoButton
          label="Reset All"
          variant="primary"
          class="is-flex-grow-1 mw-9 h-3_5 is-shadowless"
          @click="resetFilters">
          {{ $t('general.resetAll') }}
        </NeoButton>
        <NeoButton
          variant="k-accent"
          class="is-flex-grow-1 mw-9 h-3_5"
          @click="applyFilters">
          {{ $t('general.apply') }}
        </NeoButton>
      </div>
    </div>
  </NeoSidebar>
</template>

<script lang="ts" setup>
import { NeoButton, NeoSidebar } from '@kodadot1/brick'
import { useExploreFiltersStore } from '@/stores/exploreFilters'
import { useAcivityFiltersStore } from '@/stores/activityFilters'
import { usePreferencesStore } from '@/stores/preferences'
import StatusFilter from '@/components/shared/filters/modules/StatusFilter.vue'
import EventTypeFilter from '@/components/shared/filters/modules/EventTypeFilter.vue'
import PriceFilter from '@/components/shared/filters/modules/PriceFilter.vue'
import AdvancedFilter from '@/components/shared/filters/modules/AdvancedFilter.vue'
import PopularCollections from '@/components/shared/filters/modules/PopularCollections.vue'
import { getCollectionIds } from '@/utils/queryParams'
import { NeoIcon } from '@kodadot1/brick'

const route = useRoute()
const preferencesStore = usePreferencesStore()
const exploreFiltersStore = useExploreFiltersStore()
const activityFiltersStore = useAcivityFiltersStore()

const isCollectionActivityTab = computed(
  () => route.name === 'prefix-collection-id-activity',
)

const isExploreItems = computed(() => route.name === 'prefix-explore-items')

const { replaceUrl } = useReplaceUrl({
  resetPage: !isCollectionActivityTab.value,
})

const emit = defineEmits(['resetPage'])

const open = computed(() => preferencesStore.getMobileFilterCollapse)

const onClose = () => {
  syncFromUrl()
  closeFilterModal()
}

const closeFilterModal = () => preferencesStore.setMobileFilterCollapse(false)

const syncFromUrlOnActivityTab = () => {
  const sale = is(route.query?.sale?.toString()),
    offer = is(route.query?.offer?.toString()),
    listing = is(route.query?.listing?.toString()),
    mint = is(route.query?.mint?.toString()),
    transfer = is(route.query?.transfer?.toString())

  activityFiltersStore.setFilters({ sale, offer, listing, mint, transfer })
}
const syncFromUrlOnGrid = () => {
  const listed = route.query?.listed?.toString() === 'true',
    owned = route.query?.owned?.toString() === 'true',
    artView = route.query?.art_view?.toString() === 'true',
    collections = getCollectionIds()

  exploreFiltersStore.setListed(listed)
  exploreFiltersStore.setOwned(owned)
  exploreFiltersStore.setArtView(artView)
  exploreFiltersStore.setCollections(collections)
}

const syncFromUrl = () => {
  const min = Number(route.query?.min) || undefined,
    max = Number(route.query?.max) || undefined

  if (isCollectionActivityTab.value) {
    syncFromUrlOnActivityTab()
    activityFiltersStore.setPriceRange({ min, max })
  } else {
    syncFromUrlOnGrid()
    exploreFiltersStore.setPriceRange({ min, max })
  }
}

const resetFilterOnAcivityTab = () => {
  const statusDefaults = {
    sale: undefined,
    offer: undefined,
    listing: undefined,
    mint: undefined,
    transfer: undefined,
  }

  const priceDefaults = {
    min: undefined,
    max: undefined,
  }
  activityFiltersStore.setFilters(statusDefaults)

  activityFiltersStore.setPriceRange(priceDefaults)
  replaceUrl({
    ...statusDefaults,
    ...priceDefaults,
  })
}
const resetFilters = () => {
  if (isCollectionActivityTab.value) {
    resetFilterOnAcivityTab()
  } else {
    const statusDefaults = {
      listed: false,
      owned: false,
      artView: false,
      collections: undefined,
    }

    exploreFiltersStore.setListed(statusDefaults.listed)
    exploreFiltersStore.setOwned(statusDefaults.owned)
    exploreFiltersStore.setArtView(statusDefaults.artView)
    exploreFiltersStore.setCollections(statusDefaults.collections)

    // price
    const priceDefaults = {
      min: undefined,
      max: undefined,
    }
    exploreFiltersStore.setPriceRange(priceDefaults)

    replaceUrl({
      ...statusDefaults,
      ...priceDefaults,
    })
  }

  emit('resetPage')
  closeFilterModal()
}

const applyFilters = () => {
  // status filters
  const { artView, ...restStatusFilters } = exploreFiltersStore.getStatusFilters
  const priceRangeFilter = exploreFiltersStore.getPriceRange
  const eventTypeFilter = activityFiltersStore.getEventTypeFilters

  // apply to URL
  if (isCollectionActivityTab.value) {
    replaceUrl({ ...eventTypeFilter, ...priceRangeFilter })
  } else {
    replaceUrl({ art_view: artView, ...restStatusFilters, ...priceRangeFilter })
  }
  emit('resetPage')
  closeFilterModal()
}

watch(() => route.query, syncFromUrl, { immediate: true })
</script>

<style lang="scss" scoped>
@import '@/assets/styles/abstracts/variables';
.is-fullheight {
  height: 100%;
}
.is-absolute {
  position: absolute;
}
.buttons-container {
  position: sticky;
  bottom: 0;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
}

.top {
  z-index: 1000;
}
.h-3_5 {
  height: 3.5rem !important;
}
.mw-9 {
  min-width: 9rem;
}
</style>

<style lang="scss">
@import '@/assets/styles/abstracts/variables';
.background-color .o-side {
  &__content {
    @include ktheme() {
      background-color: theme('background-color');
    }
  }
  &__overlay {
    @include ktheme() {
      background-color: theme('background-color');
      opacity: 0.86;
    }
  }
}
</style>
