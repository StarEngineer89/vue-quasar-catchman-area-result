<template>

  <q-layout view="lHh Lpr lFf">

    <q-header elevated>
      <!-- <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="leftDrawerOpen = !leftDrawerOpen"
        />

        <q-toolbar-title>
          Catchment Area Results
        </q-toolbar-title>


        <div>2022</div>

      </q-toolbar> -->
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      bordered
      content-class="bg-grey-1"
    >
    <q-toolbar class="bg-primary text-white">

        <q-toolbar-title>
          Selection Options
        </q-toolbar-title>
      </q-toolbar>

      <q-select
          rounded
          outlined
          bottom-slots
          stack-label
          :dense="dense"
          v-model="stateSelected"
          :options="stateOptions"
          label="State"
          class="mystyle"
        />

        <q-select
          rounded
          outlined
          bottom-slots
          stack-label
          :dense="dense"
          v-model="geolevelSelected"
          :options="geolevelOptions"
          label="Select a geographic level"
          class="mystyle"
        />

      <q-list>
        <q-item-label
          header
          class="text-grey-8"
        >
          Options
        </q-item-label>
        <EssentialLink
          v-for="link in essentialLinks"
          :key="link.title"
          v-bind="link"
        />
      </q-list>
    </q-drawer>


    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
 import EssentialLink from 'components/EssentialLink.vue'
 import { ref } from "vue";

const linksData = [
  // {
  //   title: 'Docs',
  //   caption: 'quasar.dev',
  //   icon: 'school',
  //   link: 'https://quasar.dev'
  // },
  // {
  //   title: 'Github',
  //   caption: 'github.com/quasarframework',
  //   icon: 'code',
  //   link: 'https://github.com/quasarframework'
  // },
  // {
  //   title: 'Discord Chat Channel',
  //   caption: 'chat.quasar.dev',
  //   icon: 'chat',
  //   link: 'https://chat.quasar.dev'
  // },
  // {
  //   title: 'Forum',
  //   caption: 'forum.quasar.dev',
  //   icon: 'record_voice_over',
  //   link: 'https://forum.quasar.dev'
  // },
  // {
  //   title: 'Twitter',
  //   caption: '@quasarframework',
  //   icon: 'rss_feed',
  //   link: 'https://twitter.quasar.dev'
  // },
  // {
  //   title: 'Facebook',
  //   caption: '@QuasarFramework',
  //   icon: 'public',
  //   link: 'https://facebook.quasar.dev'
  // },
  // {
  //   title: 'Quasar Awesome',
  //   caption: 'Community Quasar projects',
  //   icon: 'favorite',
  //   link: 'https://awesome.quasar.dev'
  // }
]

export default {
  name: 'MainLayout',
  components: {
     EssentialLink
  },
  data() {
    return {
      dense: ref(false),
      denseOpts: ref(false),
     stateSelected: "",
     stateOptions: ["United States", "South Carolina"],
     geolevelSelected: "",
     geolevelOptions: [""],
     categorySelected: "",
     categoryOptions: [""],
     leftDrawerOpen: false,
       essentialLinks: linksData
    }
  }
}
</script>
