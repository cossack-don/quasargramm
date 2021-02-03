<template>
  <q-page class="constrain-more q-pa-md">
<!-- IMG -->
    <div class="camera-frame q-pa-md">

      <video v-show="!imageCaptured" class="full-width" autoplay ref="video" />

      <canvas v-show="imageCaptured" ref="canvas" class="full-width" height="240" />
    </div>
<!-- END IMG -->

<!-- BIG ICON PHOTO -->
    <div class="text-center q-pa-md">
      <q-btn v-if="hasCameraSupport" @click="captureImage" round size="lg" color="grey-10" icon="eva-camera" />
      
      <q-file v-else label="Choose an image" accept="image/*" outlined v-model="imageUpload" @input="captureImageFallBack">
        <template v-slot:prepend>
          <q-icon name="eva-attach-outline" />
        </template>
      </q-file>

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
// uid автоматическое создание id , не повторяющегося
import { uid } from 'quasar'
// для кроссбраузерности камеры мд-гум-полифил
require('md-gum-polyfill')

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
      },

      imageCaptured: false,
      imageUpload: [],
      hasCameraSupport: true,
    }
  },
      // ------------------------------------------------------------BLOCK------------------------------------------------------
  methods: {
    // метод  initCamera за показ видео( камеры для фото) и <video class="full-width" autoplay ref="video" />
    initCamera() {
      navigator.mediaDevices.getUserMedia({
        video:true
      }).then(stream => {
        this.$refs.video.srcObject = stream
      }).catch(error => {
        this.hasCameraSupport = false
      })
    },
    // ------------------------------------------------------------BLOCK------------------------------------------------------
// метод отвечает за клик по кнопке снять, отрисовка фото идет с помощью канваса
    captureImage() {
     let video = this.$refs.video
     let canvas = this.$refs.canvas

     canvas.width = video.getBoundingClientRect().width
     canvas.height = video.getBoundingClientRect().height

     let context = canvas.getContext('2d')

     context.drawImage(video, 0, 0, canvas.width, canvas.height)
    //  конец канваса

    // имдж каптура вначале показывает камеру, после клика на кнопку снять, убирается видео элемент и показывается скрин что сняли
      this.imageCaptured = true
      this.post.photo = this.dataURItoBlob(canvas.toDataURL())
// остановка работы камеры после скрина
      this.disableCamera()
    },
    // ------------------------------------------------------------BLOCK------------------------------------------------------

// метод для картинки прекрепленной если камера заблочена
    captureImageFallBack(file) {
    this.post.photo = file
    

     let canvas = this.$refs.canvas
     let context = canvas.getContext('2d')


      var reader = new FileReader();

      reader.onload = event => {
          var img = new Image();

          img.onload = () => {
            canvas.width = img.width
            canvas.height = img.height
            context.drawImage(img,0,0)
            this.imageCaptured = true
          }

          img.src = event.target.result
      }

      reader.readAsDataURL(file)
    },
    // ------------------------------------------------------------BLOCK------------------------------------------------------
    disableCamera() {
      this.$refs.video.srcObject.getVideoTracks().forEach(track => {
        track.stop()
      })
    },
    // ------------------------------------------------------------BLOCK------------------------------------------------------
    dataURItoBlob(dataURI) {
      // convert base64 to raw binary data held in a string
      // doesn't handle URLEncoded DataURIs - see SO answer #6850276 for code that does this
      var byteString = atob(dataURI.split(',')[1]);

      // separate out the mime component
      var mimeString = dataURI.split(',')[0].split(':')[1].split(';')[0]

      // write the bytes of the string to an ArrayBuffer
      var ab = new ArrayBuffer(byteString.length);

      // create a view into the buffer
      var ia = new Uint8Array(ab);

      // set the bytes of the buffer to the correct values
      for (var i = 0; i < byteString.length; i++) {
          ia[i] = byteString.charCodeAt(i);
      }

      // write the ArrayBuffer to a blob, and you're done
      var blob = new Blob([ab], {type: mimeString});
      return blob;

    }
        // ------------------------------------------------------------BLOCK------------------------------------------------------
  },

      // ------------------------------------------------------------BLOCK------------------------------------------------------
  mounted() {
    this.initCamera()
  },
  beforeDestroy() {
// если уходим на другую страницу, отключаем камеру
    if (this.hasCameraSupport) {
      this.disableCamera()
    }

  }
}
</script>

<style lang="sass">
.camera-frame
  border: $grey-10 solid 2px
  border-radius: 10px
</style>