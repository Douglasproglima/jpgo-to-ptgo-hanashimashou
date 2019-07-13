<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          @click="leftDrawerOpen = !leftDrawerOpen"
          aria-label="Menu"
        >
          <q-icon name="menu" />
        </q-btn>

        <q-toolbar-title>
          日本語とポルトガルを語話しましょう
        </q-toolbar-title>

        <div>Versão: {{ version_qspeech }}</div>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      bordered
      content-class="bg-grey-2"
      :breakpoint="400"
      show-if-above
    >
    <q-scroll-area style="height: calc(100% - 150px); margin-top: 150px; border-right: 1px solid #ddd">
      <q-list>
        <q-item-label header>Essential Links</q-item-label>
        <q-item clickable to="/" exact>
          <q-item-section avatar>
            <q-icon name="home" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Home</q-item-label>
            <q-item-label caption></q-item-label>
          </q-item-section>
        </q-item>
        <q-item clickable to="/cadastro">
          <q-item-section avatar>
            <q-icon name="settings_voice" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Register Speech</q-item-label>
            <q-item-label caption>Voice Record</q-item-label>
          </q-item-section>
        </q-item>
        <q-item clickable tag="a" target="_blank" href="https://douglasproglima.com">
          <q-item-section avatar>
            <q-icon name="business" />
          </q-item-section>
          <q-item-section>
            <q-item-label>My Website</q-item-label>
            <q-item-label caption>douglasproglima.com</q-item-label>
          </q-item-section>
        </q-item>
      </q-list>
    </q-scroll-area>
       <q-img class="absolute-top" src="statics/fundo-menu.jpg" style="height: 150px">
          <div class="absolute-bottom bg-transparent">
            <div class="text-weight-bold">Douglas Lima - リマ</div>
            <div class="q-gutter-sm">
              <q-btn dense color="white" text-color="primary" round size="sm" icon="fab fa-github"
              type="a" href="https://github.com/Douglasproglima" target="_blank" />
              <q-btn dense color="white" text-color="primary" round size="sm" icon="fab fa-linkedin-in"
              type="a" href="https://www.linkedin.com/in/douglas-lima-4bba0132/" target="_blank" />
              <q-btn dense color="white" text-color="primary" round size="sm" icon="fab fa-twitter"
              type="a" href="https://twitter.com/douglasproglima" target="_blank" />
            </div>
          </div>
        </q-img>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { openURL } from 'quasar'

export default {
  name: 'MyLayout',
  data () {
    return {
      leftDrawerOpen: this.$q.platform.is.desktop,
      version_qspeech: process.env.VERSION_QSPEECH
    }
  },
  methods: {
    openURL
  }
}
</script>

<style>
</style>
