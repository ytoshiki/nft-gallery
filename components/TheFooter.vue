<template>
  <footer class="footer-container section" data-testid="footer-container">
    <div
      class="footer-container-inner is-flex is-align-items-start is-justify-content-space-between">
      <section class="footer-container-subs is-flex is-flex-direction-column">
        <h2 class="subtitle is-5">
          {{ $t('footer.subscribe') }}
        </h2>
        <div class="is-flex is-align-items-center footer-container-subs-items">
          <CustomSubstackEmbed />
        </div>
      </section>
      <section class="footer-container-info is-flex is-flex-direction-column">
        <h2 class="subtitle is-5">Incentives</h2>
        <div>
          <ul class="footer-container-list">
            <li
              v-for="item in menuIncentives"
              :key="item.url"
              class="footer-container-info-list-item">
              <a
                v-if="item.external"
                v-safe-href="item.url"
                target="_blank"
                rel="nofollow noopener noreferrer"
                class="is-flex is-align-items-center">
                {{ item.name }}
                <NeoIcon icon="arrow-up-right" class="ml-1 has-text-grey" />
              </a>
              <nuxt-link v-else :to="item.url">
                {{ item.name }}
              </nuxt-link>
            </li>
          </ul>
        </div>
      </section>
      <section class="footer-container-info is-flex is-flex-direction-column">
        <h2 class="subtitle is-5">Marketplace</h2>
        <div>
          <ul class="footer-container-list">
            <li
              v-for="item in menuMarketplace"
              :key="item.url"
              class="footer-container-info-list-item">
              <a
                v-if="item.external"
                v-safe-href="item.url"
                target="_blank"
                rel="nofollow noopener noreferrer"
                class="is-flex is-align-items-center">
                {{ item.name }}
                <NeoIcon icon="arrow-up-right" class="ml-1 has-text-grey" />
              </a>
              <nuxt-link v-else :to="item.url">
                {{ item.name }}
              </nuxt-link>
            </li>
          </ul>
        </div>
      </section>
      <section class="footer-container-info is-flex is-flex-direction-column">
        <h2 class="subtitle is-5">KodaDot</h2>
        <div>
          <ul class="footer-container-list">
            <li
              v-for="item in menuKodadot"
              :key="item.url"
              class="footer-container-info-list-item">
              <a
                v-if="item.external"
                v-safe-href="item.url"
                target="_blank"
                rel="nofollow noopener noreferrer"
                class="is-flex is-align-items-center">
                {{ item.name }}
                <NeoIcon icon="arrow-up-right" class="ml-1 has-text-grey" />
              </a>
              <nuxt-link v-else :to="item.url">
                {{ item.name }}
              </nuxt-link>
            </li>
          </ul>
        </div>
      </section>

      <section
        class="footer-container-socials is-flex is-flex-direction-column">
        <h2 class="subtitle is-5">
          {{ $t('footer.join') }}
        </h2>
        <ul class="footer-container-socials-list is-flex">
          <li
            v-for="item in socials"
            :key="item.url"
            class="footer-container-socials-list-item is-flex is-align-items-center is-justify-content-center mr-2">
            <a
              v-safe-href="item.url"
              class="is-flex icon"
              rel="nofollow noopener noreferrer"
              target="_blank"
              role="link"
              :aria-label="item.name">
              <!-- substack doesnt have a font awesome icon -->
              <svg
                v-if="item.icon === 'substack'"
                width="16"
                height="16"
                viewBox="0 0 448 512"
                fill="none"
                xmlns="http://www.w3.org/2000/svg">
                <g clip-path="url(#clip0_6104_83750)">
                  <path
                    fill-rule="evenodd"
                    clip-rule="evenodd"
                    d="M0 0H448V62.804H0V0ZM0 229.083H448V511.471L223.954 385.808L0 511.471V229.083ZM0 114.541H448V177.345H0V114.541Z"
                    fill="currentColor" />
                </g>
                <defs>
                  <clipPath id="clip0_6104_83750">
                    <rect width="448" height="511.471" fill="currentColor" />
                  </clipPath>
                </defs>
              </svg>
              <NeoIcon
                v-else
                :pack="item.pack || item.name == 'Swag' ? 'fasr' : 'fab'"
                :icon="item.icon" />
            </a>
          </li>
        </ul>
      </section>
    </div>
    <img src="/blurred-landing-footer.png" class="left-blurred-image" alt="" />
  </footer>
</template>

<script lang="ts" setup>
// import { TranslateResult } from 'vue-i18n/types'
import { NeoIcon } from '@kodadot1/brick'

interface Menu {
  name: string
  url: string
  external?: boolean
}

const { $i18n } = useNuxtApp()

const menuIncentives: Menu[] = [
  {
    name: $i18n.t('ambassador program'),
    url: 'https://hello.kodadot.xyz/be-part-of-kodadot/kodadots-programs/ambassador-program',
    external: true,
  },
  {
    name: $i18n.t('artist ambassador'),
    url: 'https://hello.kodadot.xyz/be-part-of-kodadot/kodadots-programs/artist-ambassador-program',
    external: true,
  },

  {
    name: $i18n.t('referralProgram'),
    url: 'https://hello.kodadot.xyz/be-part-of-kodadot/kodadots-programs/artist-referral-program',
    external: true,
  },
]

const menuMarketplace: Menu[] = [
  {
    name: $i18n.t('developers'),
    url: 'https://developers.kodadot.xyz',
    external: true,
  },
  {
    name: 'FAQ',
    url: 'https://hello.kodadot.xyz/ecosystem/faq',
    external: true,
  },
  {
    name: $i18n.t('tutorial'),
    url: 'https://hello.kodadot.xyz/tutorial/',
    external: true,
  },
]

const menuKodadot: Menu[] = [
  {
    name: $i18n.t('about'),
    url: 'https://hello.kodadot.xyz/about-us/who-are-we',
    external: true,
  },
  {
    name: $i18n.t('careers'),
    url: 'https://hello.kodadot.xyz/be-part-of-kodadot/join-as-a-developer/hiring',
    external: true,
  },
  {
    name: $i18n.t('merchshop'),
    url: 'https://shop.kodadot.xyz',
    external: true,
  },

  {
    name: $i18n.t('press kit'),
    url: 'https://github.com/kodadot/kodadot-presskit/tree/main/pre-v4',
    external: true,
  },
  {
    name: $i18n.t('blog'),
    url: '/blog',
    external: false,
  },
  {
    name: $i18n.t('footer.privacyPolicy'),
    url: '/privacy-policy',
    external: false,
  },
  {
    name: $i18n.t('footer.toc'),
    url: '/terms-of-use',
    external: false,
  },
]

const socials = [
  {
    name: 'Twitter',
    url: 'https://twitter.com/KodaDot',
    icon: 'x-twitter',
  },
  {
    name: 'Beehiiv',
    url: 'https://kodadotweeklyroundup.beehiiv.com',
    icon: 'newspaper',
    pack: 'fal',
  },
  {
    name: 'Linkedin',
    url: 'https://www.linkedin.com/company/kodadot',
    icon: 'linkedin',
  },
  {
    name: 'Medium',
    url: 'https://medium.com/kodadot',
    icon: 'medium',
  },
  {
    name: 'Youtube',
    url: 'https://www.youtube.com/channel/UCEULduld5NrqOL49k1KVjoA/',
    icon: 'youtube',
  },

  {
    name: 'Instagram',
    url: 'https://instagram.com/kodadot.xyz',
    icon: 'instagram',
  },
  {
    name: 'Reddit',
    url: 'https://www.reddit.com/r/KodaDot',
    icon: 'reddit-alien',
  },
]
</script>
