<template>
  <q-layout view="lHh Lpr lFf">

    <!-- Header -->
    <q-header class="bg-white text-grey-10" bordered>
      <q-toolbar class="constrain">
<!-- btn -->
        <q-btn class="large-screen-only q-mr-sm"
         to="/camera" flat round dense 
         icon="eva-camera-outline" size="18px" />

        <q-separator vertical spaced class="large-screen-only" />
<!-- title app -->
        <q-toolbar-title class="text-grand-hotel text-bold">
          Quasargramm App
        </q-toolbar-title>
<!-- btn -->
        <q-btn class="large-screen-only"
        to="/" flat round dense 
        icon="eva-home-outline" size="18px" />
        
      </q-toolbar>
    </q-header>
<!-- end Header -->

<!-- footer -->
    <q-footer class="small-screen-only bg-white" bordered>
    <!-- all tabs -->
        <q-tabs class="text-grey-10" active-color="primary" indicator-color="transparent">
          <!-- tab-item icon-route -->
        <q-route-tab to="/" icon="eva-home-outline" />
        <q-route-tab to="/camera" icon="eva-camera-outline" />

        </q-tabs>
      
    </q-footer>
<!-- end footer -->

<!-- main content pages -->
    <q-page-container class="bg-grey-1">
      <router-view />
    </q-page-container>
<!-- end main content pages -->
  </q-layout>
</template>

<script>


export default {
  name: 'MainLayout',

  data () {
    return {

    }
  }
}
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
  .q-tab__icon
    font-size: 30px
</style>