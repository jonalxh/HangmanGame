<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          @click="leftDrawerOpen = !leftDrawerOpen"
          icon="menu"
          aria-label="Menu"
        />

        <q-toolbar-title>
          Ahorcado
        </q-toolbar-title>

      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      bordered
      content-class="bg-grey-1"
    >
      <q-list>
        <q-item-label header class="text-grey-8">Menu principal</q-item-label>
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
import EssentialLink from 'components/EssentialLink'

export default {
  name: 'MainLayout',

  components: {
    EssentialLink
  },

  data () {
    return {
      leftDrawerOpen: false,
      essentialLinks: [
        {
          title: 'Palabras',
          icon: 'menu_book',
          link: 'https://quasar.dev'
        },
        {
          title: 'Niveles',
          icon: 'trending_up',
          link: 'https://quasar.dev'
        },
        {
          title: 'Tus partidas',
          icon: 'playlist_play',
          link: 'https://quasar.dev'
        },
        {
          title: 'Perfil',
          icon: 'person_outline',
          link: 'https://quasar.dev'
        }
      ]
    }
  }
}
</script>
