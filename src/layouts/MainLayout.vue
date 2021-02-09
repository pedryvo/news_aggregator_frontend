<template>
  <q-layout view="lHh Lpr lFf">
    <q-header class="bg-white text-grey-10" bordered>
      <q-toolbar class="constrain">
        <q-icon style="font-size: 32px;" name="article" />
        <q-separator vertical spaced class="large-screen-only"/>
        <q-toolbar-title class="text-grand-hotel text-bold justify-center text-center">
           Vca News
        </q-toolbar-title>
      </q-toolbar>
    </q-header>

    <q-footer class="bg-white" bordered>
      <q-tabs class="text-grey-10" active-color="primary" indicator-color="transparent">
        <q-route-tab @click="reloadPage" to="/" icon="home" />
      </q-tabs>
    </q-footer>

    <q-page-container class="bg-grey-1">
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
export default {
  name: "MainLayout",

  data() {
    return {};
  },
  methods: {
    reloadPage(){
      window.location.reload()
    }
  }
};
</script>

<style lang="sass">
.q-toolbar
  @media (min-width: $breakpoint-sm-min)
    height: 77px
.q-toolbar__title
  font-size: 30px
  @media (max-width: $breakpoint-xs-max)
    text-align: center
  .q-footer
    .q-tab-icon
      font-size: 30px
</style>
