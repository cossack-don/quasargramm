<template>
  <q-page class="constrain-more q-pa-md">
<!-- IMG -->
    <div class="camera-frame q-pa-md">
      <img class="full-width" src="https://cdn.quasar.dev/img/parallax2.jpg">
    </div>
<!-- END IMG -->

<!-- BIG ICON PHOTO -->
    <div class="text-center q-pa-md">
      <q-btn round size="lg" color="grey-10" icon="eva-camera" />
    </div>      
<!-- END BIG ICON PHOTO -->

<!-- inputs caption location -->
    <div class="row justify-center q-ma-md">
        <q-input dense v-model="post.caption" label="Caption" class="col col-sm-6" />
    </div>

    <div class="row justify-center q-ma-md">
        <q-input dense v-model="post.location" label="Location" class="col col-sm-6">

        <template v-slot:append>
          <q-btn round dense flat icon="eva-navigation-2-outline" />
        </template>

        </q-input>
    </div>
<!--end inputs caption location -->

<!-- btn post -->
      <div class="row justify-center q-mt-lg">
        <q-btn unelevated rounded color="primary" label="Post Image" />
      </div>
<!--end btn post -->
  </q-page>
</template>

<script>
import { uid } from 'quasar'

export default {
  name: 'PageCamera',
  data() {
    return {
      post: {
        id:uid(),
        caption:'',
        location: '',
        photo:null,
        date:Date.now()
      }
    }
  }
}
</script>

<style lang="sass">
.camera-frame
  border: $grey-10 solid 2px
  border-radius: 10px
</style>