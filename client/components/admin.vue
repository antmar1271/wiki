<template lang='pug'>
  v-app.admin
    nav-header(hide-search)
      template(slot='mid')
        v-spacer
        .overline.grey--text {{$t('admin:adminArea')}}
        v-spacer
    v-navigation-drawer.pb-0.admin-sidebar(v-model='adminDrawerShown', app, fixed, clipped, :right='$vuetify.rtl', permanent, width='300')
      vue-scroll(:ops='scrollStyle')
        v-list(dense, nav)
          v-list-item(to='/dashboard')
            v-list-item-avatar(size='24'): v-icon mdi-view-dashboard-variant
            v-list-item-title {{ $t('admin:dashboard.title') }}
          template(v-if='hasPermission([`manage:system`, `manage:navigation`, `write:pages`, `manage:pages`, `delete:pages`])')
            v-divider.my-2
            v-subheader.pl-4 {{ $t('admin:nav.site') }}
            v-list-item(to='/general', v-if='hasPermission(`manage:system`)')
              v-list-item-avatar(size='24'): v-icon mdi-widgets
              v-list-item-title {{ $t('admin:general.title') }}
            v-list-item(to='/locale', v-if='hasPermission(`manage:system`)')
              v-list-item-avatar(size='24'): v-icon mdi-web
              v-list-item-title {{ $t('admin:locale.title') }}
            v-list-item(to='/navigation', v-if='hasPermission([`manage:system`, `manage:navigation`])')
              v-list-item-avatar(size='24'): v-icon mdi-near-me
              v-list-item-title {{ $t('admin:navigation.title') }}
            v-list-item(to='/pages', v-if='hasPermission([`manage:system`, `write:pages`, `manage:pages`, `delete:pages`])')
              v-list-item-avatar(size='24'): v-icon mdi-file-document-outline
              v-list-item-title {{ $t('admin:pages.title') }}
              v-list-item-action.pr-3
                v-chip(x-small, :color='darkMode ? `grey darken-3-d4` : `grey lighten-5`')
                  .caption.grey--text {{ info.pagesTotal }}
            v-list-item(to='/theme', v-if='hasPermission([`manage:system`, `manage:theme`])')
              v-list-item-avatar(size='24'): v-icon mdi-palette-outline
              v-list-item-title {{ $t('admin:theme.title') }}
          template(v-if='hasPermission([`manage:system`, `manage:groups`, `write:groups`, `manage:users`, `write:users`])')
            v-divider.my-2
            v-subheader.pl-4 {{ $t('admin:nav.users') }}
            v-list-item(to='/groups', v-if='hasPermission([`manage:system`, `manage:groups`, `write:groups`])')
              v-list-item-avatar(size='24'): v-icon mdi-account-group
              v-list-item-title {{ $t('admin:groups.title') }}
              v-list-item-action.pr-3
                v-chip(x-small, :color='darkMode ? `grey darken-3-d4` : `grey lighten-4`')
                  .caption.grey--text {{ info.groupsTotal }}
            v-list-item(to='/users', v-if='hasPermission([`manage:system`, `manage:groups`, `write:groups`, `manage:users`, `write:users`])')
              v-list-item-avatar(size='24'): v-icon mdi-account-box
              v-list-item-title {{ $t('admin:users.title') }}
              v-list-item-action.pr-3
                v-chip(x-small, :color='darkMode ? `grey darken-3-d4` : `grey lighten-4`')
                  .caption.grey--text {{ info.usersTotal }}
          template(v-if='hasPermission(`manage:system`)')
            v-divider.my-2
            v-subheader.pl-4 {{ $t('admin:nav.modules') }}
            v-list-item(to='/analytics')
              v-list-item-avatar(size='24'): v-icon mdi-chart-timeline-variant
              v-list-item-title {{ $t('admin:analytics.title') }}
            v-list-item(to='/auth')
              v-list-item-avatar(size='24'): v-icon mdi-lock-outline
              v-list-item-title {{ $t('admin:auth.title') }}
            v-list-item(to='/comments', disabled)
              v-list-item-avatar(size='24'): v-icon(color='grey lighten-2') mdi-comment-text-outline
              v-list-item-title {{ $t('admin:comments.title') }}
            v-list-item(to='/editor', disabled)
              v-list-item-avatar(size='24'): v-icon(color='grey lighten-2') mdi-playlist-edit
              v-list-item-title {{ $t('admin:editor.title') }}
            v-list-item(to='/logging', disabled)
              v-list-item-avatar(size='24'): v-icon(color='grey lighten-2') mdi-script-text-outline
              v-list-item-title {{ $t('admin:logging.title') }}
            v-list-item(to='/rendering')
              v-list-item-avatar(size='24'): v-icon mdi-cogs
              v-list-item-title {{ $t('admin:rendering.title') }}
            v-list-item(to='/search')
              v-list-item-avatar(size='24'): v-icon mdi-cloud-search-outline
              v-list-item-title {{ $t('admin:search.title') }}
            v-list-item(to='/storage')
              v-list-item-avatar(size='24'): v-icon mdi-harddisk
              v-list-item-title {{ $t('admin:storage.title') }}
          template(v-if='hasPermission([`manage:system`, `manage:api`])')
            v-divider.my-2
            v-subheader.pl-4 {{ $t('admin:nav.system') }}
            v-list-item(to='/api', v-if='hasPermission([`manage:system`, `manage:api`])', disabled)
              v-list-item-avatar(size='24'): v-icon(color='grey lighten-2') mdi-call-split
              v-list-item-title {{ $t('admin:api.title') }}
            v-list-item(to='/mail', v-if='hasPermission(`manage:system`)')
              v-list-item-avatar(size='24'): v-icon mdi-email-multiple-outline
              v-list-item-title {{ $t('admin:mail.title') }}
            v-list-item(to='/system', v-if='hasPermission(`manage:system`)')
              v-list-item-avatar(size='24'): v-icon mdi-tune
              v-list-item-title {{ $t('admin:system.title') }}
            v-list-item(to='/utilities', v-if='hasPermission(`manage:system`)')
              v-list-item-avatar(size='24'): v-icon mdi-wrench-outline
              v-list-item-title {{ $t('admin:utilities.title') }}
            v-list-item(to='/webhooks', v-if='hasPermission(`manage:system`)', disabled)
              v-list-item-avatar(size='24'): v-icon(color='grey lighten-2') mdi-webhook
              v-list-item-title {{ $t('admin:webhooks.title') }}
            v-list-group(
              to='/dev'
              no-action
              v-if='hasPermission([`manage:system`, `manage:api`])'
              )
              v-list-item(slot='activator')
                v-list-item-avatar(size='24'): v-icon mdi-dev-to
                v-list-item-title {{ $t('admin:dev.title') }}

              v-list-item(to='/dev-flags')
                v-list-item-title {{ $t('admin:dev.flags.title') }}
              v-list-item(to='/dev-graphiql')
                v-list-item-title {{ $t('admin:dev.graphiql.title') }}
              v-list-item(to='/dev-voyager')
                v-list-item-title {{ $t('admin:dev.voyager.title') }}
            v-divider.my-2
          v-list-item(to='/contribute')
            v-list-item-avatar(size='24'): v-icon mdi-heart-outline
            v-list-item-title {{ $t('admin:contribute.title') }}

    v-content(:class='darkMode ? "grey darken-4" : ""')
      transition(name='admin-router')
        router-view

    nav-footer
    notify
    search-results
</template>

<script>
import _ from 'lodash'
import VueRouter from 'vue-router'
import { get, sync } from 'vuex-pathify'

import statsQuery from 'gql/admin/dashboard/dashboard-query-stats.gql'

import adminStore from '@/store/admin'

/* global WIKI */

WIKI.$store.registerModule('admin', adminStore)

const router = new VueRouter({
  mode: 'history',
  base: '/a',
  routes: [
    { path: '/', redirect: '/dashboard' },
    { path: '/dashboard', component: () => import(/* webpackChunkName: "admin" */ './admin/admin-dashboard.vue') },
    { path: '/general', component: () => import(/* webpackChunkName: "admin" */ './admin/admin-general.vue') },
    { path: '/locale', component: () => import(/* webpackChunkName: "admin" */ './admin/admin-locale.vue') },
    { path: '/navigation', component: () => import(/* webpackChunkName: "admin" */ './admin/admin-navigation.vue') },
    { path: '/pages', component: () => import(/* webpackChunkName: "admin" */ './admin/admin-pages.vue') },
    { path: '/pages/:id', component: () => import(/* webpackChunkName: "admin" */ './admin/admin-pages-edit.vue') },
    { path: '/theme', component: () => import(/* webpackChunkName: "admin" */ './admin/admin-theme.vue') },
    { path: '/groups', component: () => import(/* webpackChunkName: "admin" */ './admin/admin-groups.vue') },
    { path: '/groups/:id', component: () => import(/* webpackChunkName: "admin" */ './admin/admin-groups-edit.vue') },
    { path: '/users', component: () => import(/* webpackChunkName: "admin" */ './admin/admin-users.vue') },
    { path: '/users/:id', component: () => import(/* webpackChunkName: "admin" */ './admin/admin-users-edit.vue') },
    { path: '/analytics', component: () => import(/* webpackChunkName: "admin" */ './admin/admin-analytics.vue') },
    { path: '/auth', component: () => import(/* webpackChunkName: "admin" */ './admin/admin-auth.vue') },
    { path: '/rendering', component: () => import(/* webpackChunkName: "admin" */ './admin/admin-rendering.vue') },
    { path: '/editor', component: () => import(/* webpackChunkName: "admin" */ './admin/admin-editor.vue') },
    { path: '/logging', component: () => import(/* webpackChunkName: "admin" */ './admin/admin-logging.vue') },
    { path: '/search', component: () => import(/* webpackChunkName: "admin" */ './admin/admin-search.vue') },
    { path: '/storage', component: () => import(/* webpackChunkName: "admin" */ './admin/admin-storage.vue') },
    { path: '/api', component: () => import(/* webpackChunkName: "admin" */ './admin/admin-api.vue') },
    { path: '/mail', component: () => import(/* webpackChunkName: "admin" */ './admin/admin-mail.vue') },
    { path: '/system', component: () => import(/* webpackChunkName: "admin" */ './admin/admin-system.vue') },
    { path: '/utilities', component: () => import(/* webpackChunkName: "admin" */ './admin/admin-utilities.vue') },
    { path: '/webhooks', component: () => import(/* webpackChunkName: "admin" */ './admin/admin-webhooks.vue') },
    { path: '/dev-flags', component: () => import(/* webpackChunkName: "admin-dev" */ './admin/admin-dev-flags.vue') },
    { path: '/dev-graphiql', component: () => import(/* webpackChunkName: "admin-dev" */ './admin/admin-dev-graphiql.vue') },
    { path: '/dev-voyager', component: () => import(/* webpackChunkName: "admin-dev" */ './admin/admin-dev-voyager.vue') },
    { path: '/contribute', component: () => import(/* webpackChunkName: "admin" */ './admin/admin-contribute.vue') }
  ]
})

export default {
  i18nOptions: { namespaces: 'admin' },
  data() {
    return {
      adminDrawerShown: true,
      scrollStyle: {
        vuescroll: {},
        scrollPanel: {
          initialScrollY: 0,
          initialScrollX: 0,
          scrollingX: false,
          easing: 'easeOutQuad',
          speed: 1000,
          verticalNativeBarPos: this.$vuetify.rtl ? `left` : `right`
        },
        rail: {
          gutterOfEnds: '2px'
        },
        bar: {
          onlyShowBarOnScroll: false,
          background: '#CCC',
          hoverStyle: {
            background: '#999'
          }
        }
      }
    }
  },
  computed: {
    darkMode: get('site/dark'),
    info: sync('admin/info'),
    permissions: get('user/permissions')
  },
  router,
  created() {
    this.$store.commit('page/SET_MODE', 'admin')
  },
  methods: {
    hasPermission(prm) {
      if (_.isArray(prm)) {
        return _.some(prm, p => {
          return _.includes(this.permissions, p)
        })
      } else {
        return _.includes(this.permissions, prm)
      }
    }
  },
  apollo: {
    info: {
      query: statsQuery,
      fetchPolicy: 'network-only',
      manual: true,
      result({ data, loading, networkStatus }) {
        this.info = data.system.info
      },
      watchLoading (isLoading) {
        this.$store.commit(`loading${isLoading ? 'Start' : 'Stop'}`, 'admin-stats-refresh')
      }
    }
  }
}
</script>

<style lang='scss'>

.admin {
  &.theme--light .application--wrap {
    background-color: lighten(mc('grey', '200'), 2%);
  }
}

.admin-router {
  &-enter-active, &-leave-active {
    transition: opacity .25s ease;
    opacity: 1;
  }
  &-enter-active {
    transition-delay: .25s;
  }
  &-enter, &-leave-to {
    opacity: 0;
  }
}

.admin-sidebar {
  .v-list__tile--active {
    background-color: rgba(mc('theme', 'primary'), .1);

    .v-icon {
      color: mc('theme', 'primary');
    }
  }
}

.theme--dark {
  .admin-sidebar .v-list__tile--active {
    background-color: rgba(0,0,0, .2);
    color: mc('blue', '500') !important;

    .v-icon {
      color: mc('blue', '500');
    }
  }
}

.admin-header {
  display: flex;
  justify-content: flex-start;
  align-items: center;

  &-title {
    margin-left: 1rem;
  }
}

</style>
