<template lang='pug'>
  v-container.admin-contribute(fluid, grid-list-lg)
    v-layout(row, wrap)
      v-flex(xs12)
        .admin-header
          img.animated.fadeInUp(src='/svg/icon-heart-health.svg', alt='Contribute', style='width: 80px;')
          .admin-header-title
            .headline.primary--text.animated.fadeInLeft {{ $t('admin:contribute.title') }}
            .subtitle-1.grey--text.animated.fadeInLeft.wait-p4s {{ $t('admin:contribute.subtitle') }}
        v-card.mt-3.animated.fadeInUp
          v-card-text
            i18next.body-2.pl-3(path='admin:contribute.openSource', tag='div')
              v-icon(color='red') mdi-heart
              a(href='https://requarks.io', target='_blank') requarks.io
              a(href='https://github.com/Requarks/wiki/graphs/contributors', target='_blank') {{ $t('admin:contribute.openSourceContributors') }}
            .body-2.pt-3.pl-3 {{ $t('admin:contribute.needYourHelp') }}
            v-divider.mt-3
            v-subheader.subtitle-2 {{ $t('admin:contribute.fundOurWork') }}
            v-tabs.mx-3.radius-7.admin-contribute-tabs(
              centered
              fixed-tabs
              background-color='primary'
              color='white'
              dark
              slider-color='#FFF'
              icons-and-text
              )
              v-tab
                span Patreon
                img.my-1(src='/svg/icon-patreon.svg', style='height: 24px;')
              v-tab
                span OpenCollective
                img.my-1(src='/svg/icon-opencollective.svg', style='height: 24px;')
              v-tab
                span PayPal
                img.my-1(src='/svg/icon-paypal.svg', style='height: 24px;')
              v-tab
                span Ethereum
                img.my-1(src='/svg/icon-ethereum.svg', style='height: 24px;')
              v-tab
                span T-Shirts
                img.my-1(src='/svg/icon-t-shirt.svg', style='height: 24px;')
              v-tab-item(:transition='false', :reverse-transition='false')
                .body-2.pa-3 {{ $t('admin:contribute.patreon') }}
                a.ml-3(href='https://www.patreon.com/bePatron?u=16744039', :title='$t(`admin:contribute.becomeAPatron`)')
                  img(src='/img/donate_patreon.png', :alt='$t(`admin:contribute.becomeAPatron`)' style='width:200px;')
              v-tab-item(:transition='false', :reverse-transition='false')
                .body-2.pa-3 {{ $t('admin:contribute.openCollective') }}
                a.ml-3(href='https://opencollective.com/wikijs/donate', :title='$t(`admin:contribute.makeADonation`)')
                  img(src='/img/donate_opencollective.png', :alt='$t(`admin:contribute.makeADonation`)' style='width:300px;')
              v-tab-item(:transition='false', :reverse-transition='false')
                .body-2.pa-3 {{ $t('admin:contribute.paypal') }}
                .ml-3
                  form(action='https://www.paypal.com/cgi-bin/webscr', method='post', target='_top')
                    input(type='hidden', name='cmd', value='_s-xclick')
                    input(type='hidden', name='hosted_button_id', value='FLV5X255Z9CJU')
                    input(type='image', src='/img/donate_paypal.png', border='0', name='submit', title='PayPal - The safer, easier way to pay online!', alt='Donate with PayPal button')
                    img(alt='', border='0', src='https://www.paypal.com/en_CA/i/scr/pixel.gif', width='1', height='1')
              v-tab-item(:transition='false', :reverse-transition='false')
                .body-1.pa-3 {{ $t('admin:contribute.ethereum') }}
                .ml-3
                  .admin-contribute-ethaddress
                    strong Ethereum Address
                    span 0xE1d55C19aE86f6Bcbfb17e7f06aCe96BdBb22Cb5
                  div: img(src='/img/donate_eth_qr.png')
              v-tab-item(:transition='false', :reverse-transition='false')
                .body-1.pa-3 {{ $t('admin:contribute.tshirts') }}
                v-card-actions.ml-2
                  v-btn(outline, :color='darkMode ? `blue lighten-1` : `primary`', href='https://wikijs.threadless.com', large)
                    v-icon(left) mdi-tshirt-crew
                    span {{ $t('admin:contribute.shop') }}
            v-divider.mt-3
            v-subheader.subtitle-2  {{ $t('admin:contribute.contribute') }}
            .body-2.pl-3
              ul
                i18next(path='admin:contribute.submitAnIdea', tag='li')
                  a(href='https://wiki.js.org/feedback', target='_blank') {{ $t('admin:contribute.submitAnIdeaLink') }}
                i18next(path='admin:contribute.foundABug', tag='li')
                  a(href='https://github.com/Requarks/wiki/issues', target='_blank') Github
                i18next(path='admin:contribute.helpTranslate', tag='li')
                  a(href='https://wiki.requarks.io/slack', target='_blank') Slack
            v-divider.mt-3
            v-subheader.subtitle-2  {{ $t('admin:contribute.spreadTheWord') }}
            .body-2.pl-3
              ul
                li {{ $t('admin:contribute.talkToFriends') }}
                i18next(path='admin:contribute.followUsOnTwitter', tag='li')
                  a(href='https://twitter.com/requarks', target='_blank') Twitter
          v-toolbar(color='teal', dense, dark)
            .subtitle-1 Sponsors
            v-spacer
            v-btn(outlined, small, href='https://opencollective.com/wikijs/order/1273') Become a Sponsor
          v-list(two-line)
            template(v-for='(sponsor, idx) in sponsors')
              v-list-item(:key='sponsor.id')
                v-list-item-avatar
                  img(v-if='sponsor.image', :src='sponsor.image')
                  v-avatar(v-else, color='teal', size='40')
                    span.white--text.subtitle-1 {{sponsor.name[0].toUpperCase()}}
                v-list-item-content
                  v-list-item-title {{sponsor.name}}
                  v-list-item-subtitle {{sponsor.description}}
                v-list-item-action(v-if='sponsor.twitter')
                  v-btn(icon, :href='sponsor.twitter', target='_blank')
                    v-icon mdi-twitter
                v-list-item-action(v-if='sponsor.website')
                  v-btn(icon, :href='sponsor.website', target='_blank')
                    v-icon(color='grey') mdi-earth
              v-divider(v-if='idx < sponsors.length - 1')
          v-toolbar(color='blue-grey', dense, dark)
            .subtitle-1 Backers
            v-spacer
            v-btn(outlined, small, href='https://opencollective.com/wikijs/order/1272') Become a Backer
          v-list(two-line, dense)
            template(v-for='(backer, idx) in backers')
              v-list-item(:key='backer.id')
                v-list-item-avatar
                  img(v-if='backer.image', :src='backer.image')
                  v-avatar(v-else, color='blue-grey', size='40')
                    span.white--text.subtitle-1 {{backer.name[0].toUpperCase()}}
                v-list-item-content
                  v-list-item-title {{backer.name}}
                  v-list-item-subtitle {{backer.description}}
                v-list-item-action(v-if='backer.twitter')
                  v-btn(icon, :href='backer.twitter', target='_blank')
                    v-icon mdi-twitter
                v-list-item-action(v-if='backer.website')
                  v-btn(icon, :href='backer.website', target='_blank')
                    v-icon(color='grey') mdi-earth
              v-divider(v-if='idx < backers.length - 1')
          v-toolbar(color='primary', dense, dark)
            .subtitle-1 Special Thanks
          v-list(two-line)
            v-list-item
              v-list-item-avatar
                img(src='https://static.requarks.io/logo/algolia.svg', alt='Algolia')
              v-list-item-content
                v-list-item-title Algolia
                v-list-item-subtitle Algolia is a powerful search-as-a-service solution, made easy to use with API clients, UI libraries, and pre-built integrations.
              v-list-item-action
                v-btn(icon, href='https://www.algolia.com/', target='_blank')
                  v-icon(color='grey') mdi-earth
            v-divider
            v-list-item
              v-list-item-avatar
                img(src='https://static.requarks.io/logo/browserstack.svg', alt='Browserstack')
              v-list-item-content
                v-list-item-title BrowserStack
                v-list-item-subtitle BrowserStack is a cloud web and mobile testing platform that enables developers to test their websites and mobile applications.
              v-list-item-action
                v-btn(icon, href='https://www.browserstack.com/', target='_blank')
                  v-icon(color='grey') mdi-earth
            v-divider
            v-list-item
              v-list-item-avatar
                img(src='https://static.requarks.io/logo/cloudflare.svg', alt='Cloudflare')
              v-list-item-content
                v-list-item-title Cloudflare
                v-list-item-subtitle Providing content delivery network services, DDoS mitigation, Internet security and distributed domain name server services.
              v-list-item-action
                v-btn(icon, href='https://www.cloudflare.com/', target='_blank')
                  v-icon(color='grey') mdi-earth
            v-divider
            v-list-item
              v-list-item-avatar
                img(src='https://static.requarks.io/logo/digitalocean.svg', alt='DigitalOcean')
              v-list-item-content
                v-list-item-title DigitalOcean
                v-list-item-subtitle Providing developers and businesses a reliable, easy-to-use cloud computing platform of virtual servers (Droplets), object storage (Spaces), and more.
              v-list-item-action
                v-btn(icon, href='https://m.do.co/c/5f7445bfa4d0', target='_blank')
                  v-icon(color='grey') mdi-earth
            v-divider
            v-list-item
              v-list-item-avatar(tile)
                img(src='/svg/logo-icons8.svg', alt='Icons8')
              v-list-item-content
                v-list-item-title Icons8
                v-list-item-subtitle All the Icons You Need. Guaranteed.
              v-list-item-action
                v-btn(icon, href='https://icons8.com', target='_blank')
                  v-icon(color='grey') mdi-earth
            v-divider
            v-list-item
              v-list-item-avatar(tile)
                img(src='https://static.requarks.io/logo/lokalise.png', alt='Lokalise')
              v-list-item-content
                v-list-item-title Lokalise
                v-list-item-subtitle Lokalise is a translation management system built for agile teams who want to automate their localization process.
              v-list-item-action
                v-btn(icon, href='https://lokalise.co', target='_blank')
                  v-icon(color='grey') mdi-earth
            v-divider
            v-list-item
              v-list-item-avatar(tile)
                img(src='https://static.requarks.io/logo/netlify.svg', alt='Netlify')
              v-list-item-content
                v-list-item-title Netlify
                v-list-item-subtitle Deploy modern static websites with Netlify. Get CDN, Continuous deployment, 1-click HTTPS, and all the services you need.
              v-list-item-action
                v-btn(icon, href='https://wwwnetlify.com', target='_blank')
                  v-icon(color='grey') mdi-earth

</template>

<script>
import _ from 'lodash'
import { get } from 'vuex-pathify'

import groupsQuery from 'gql/admin/contribute/contribute-query-contributors.gql'

export default {
  data() {
    return {
      contributors: []
    }
  },
  computed: {
    darkMode: get('site/dark'),
    sponsors() {
      return _.filter(this.contributors, ['tier', 'sponsors'])
    },
    backers() {
      return _.reject(this.contributors, ['tier', 'sponsors'])
    }
  },
  apollo: {
    contributors: {
      query: groupsQuery,
      fetchPolicy: 'network-only',
      update: (data) => data.contribute.contributors,
      watchLoading (isLoading) {
        this.$store.commit(`loading${isLoading ? 'Start' : 'Stop'}`, 'admin-contribute-refresh')
      }
    }
  }
}
</script>

<style lang='scss'>
.admin-contribute {

  &-tabs {
    .v-tabs__item img {
      height: 24px;
      margin-bottom: 5px;
    }
  }

  &-ethaddress {
    display: inline-block;
    margin-bottom: 12px;
    border-radius: 7px;
    background-color: mc('grey', '100');
    color: mc('grey', '700');
    padding: 12px;

    strong {
      display: block;
    }
  }

  ul {
    margin-left: 1rem;
    list-style-type: square;
  }
}
</style>
