<template>
  <div
    class="is-flex is-flex-direction-column"
    :class="[`nft-media-info__${variant}`]">
    <div
      class="is-flex is-flex-direction-column mb-2"
      :class="{ 'px-3 pt-4 staked-primary-title': !isMinimal }">
      <div class="is-flex is-justify-content-space-between">
        <span
          class="is-ellipsis has-text-weight-bold"
          data-testid="nft-name"
          :title="token.name">
          {{ token.name || '--' }}
        </span>
        <span v-if="!isMinimal">x{{ token.supply }}</span>
      </div>

      <div v-if="!isMinimal" class="is-size-7 has-text-grey">
        {{ $t('lowestPrice') }}:
        <CommonTokenMoney
          :value="token.cheapest?.price"
          data-testid="card-money" />
      </div>
    </div>

    <div
      class="is-flex is-justify-content-space-between is-align-items-center"
      :class="{ 'border-top card-border-color pt-2 px-3': !isMinimal }">
      <template v-if="!isMinimal">
        <a class="is-ellipsis pr-1" :v-safe-href="collectionUrl">
          {{ collectionNameLabel }}
        </a>
        <NeoButton
          no-shadow
          variant="text"
          tag="a"
          :v-safe-href="collectionUrl"
          class="is-size-7 nft-info-collection-name"
          label="Visit"
          icon="arrow-right" />
      </template>

      <template v-else>
        <CollectionDetailsPopover
          v-if="collectionNameLabel"
          :show-delay="collectionPopoverShowDelay"
          class="is-size-7 nft-info-collection-name is-ellipsis"
          :nft="token">
          <template #content>
            <a
              :v-safe-href="`/${prefix}/collection/${token.collection.id}`"
              class="nft-info-collection-name">
              {{ collectionNameLabel }}
            </a>
          </template>
        </CollectionDetailsPopover>

        <span>x{{ token.supply }}</span>
      </template>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { computed } from 'vue'
import { NeoButton, NftCardVariant } from '@kodadot1/brick'

import CommonTokenMoney from '@/components/shared/CommonTokenMoney.vue'
import { NeoNFT } from './types'

const props = withDefaults(
  defineProps<{
    token: NeoNFT
    prefix: string
    collectionPopoverShowDelay?: number

    variant?: NftCardVariant
  }>(),
  {
    variant: 'primary',
    collectionPopoverShowDelay: 500,
  },
)

const isMinimal = computed(() =>
  props.variant ? props.variant.includes('minimal') : false,
)

const collectionUrl = computed(
  () => `/${props.prefix}/collection/${props.token.collection.id}`,
)

const collectionNameLabel = computed(() => props.token.collection.name || '--')
</script>

<style lang="scss" scoped>
@import './NeoNftCard.scss';

.staked-primary-title {
  margin-top: 6px;
}

.nft-media-info__stacked-minimal {
  margin-top: 9px;
  padding: 9px;
  padding-bottom: 0;
  min-height: 75px;
}
</style>
