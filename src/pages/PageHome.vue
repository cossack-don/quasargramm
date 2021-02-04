<template>
  <q-page class="constrain q-pa-md">

  <div class="row q-col-gutter-lg">
    <!-- COL 8 -->
    <div class="col-12 col-sm-8">
      <template v-if="!loadingPosts && posts.length">
      <q-card class="card-post q-mb-md" flat bordered v-for="post in posts" :key="post.id">

            <q-item>
              <q-item-section avatar>
                <q-avatar>
                  <img src="https://cdn.quasar.dev/img/boy-avatar.png">
                </q-avatar>
              </q-item-section>

              <q-item-section>
                <q-item-label class="text-bold">sergey__zhur</q-item-label>
                <q-item-label caption>
                  {{post.location}}
                </q-item-label>
              </q-item-section>
            </q-item>

            <q-separator />

            <q-img
              :src="post.imageUrl"
            />

            <q-card-section>
              <div class="text-h6">{{post.caption}}</div>
              <div class="text-caption text-grey">{{post.date | niceDate}}</div>
            </q-card-section>

          </q-card>
      </template>

      <template v-else-if="!loadingPosts && !posts.length">
        <h5 class="text-center text-grey">No post yet.</h5>
      </template>
<!-- preloader если посты не загрузились сразу -->
      <template v-else>
        <q-card flat bordered>
              <q-item>
                <q-item-section avatar>
                  <q-skeleton type="QAvatar" animation="fade" size="40px" />
                </q-item-section>

                <q-item-section>
                  <q-item-label>
                    <q-skeleton type="text" animation="fade" />
                  </q-item-label>
                  <q-item-label caption>
                    <q-skeleton type="text" animation="fade" />
                  </q-item-label>
                </q-item-section>
              </q-item>

              <q-skeleton height="200px" square animation="fade" />

              <q-card-section>
                <q-skeleton type="text" class="text-subtitle2" animation="fade" />
                <q-skeleton type="text" width="50%" class="text-subtitle2" animation="fade" />
              </q-card-section>
            </q-card>
      </template>

    </div>


    <!-- COL 4 -->
    <div class="col-4 large-screen-only">
      <q-item class="fixed">
          <q-item-section avatar>
            <q-avatar size="48px">
              <img src="https://cdn.quasar.dev/img/boy-avatar.png">
            </q-avatar>
          </q-item-section>

          <q-item-section>
            <q-item-label class="text-bold">sergey__zhur</q-item-label>
            <q-item-label caption>
              Danny Connel
            </q-item-label>
          </q-item-section>
    </q-item>
    </div>
  </div>


  </q-page>
</template>

<script>
import { date } from 'quasar'

export default {
  name: 'PageHome',
  data() {
    return {
      posts: [
        // {
        //   id:1,
        //   caption: 'Golden Gate Bridge',
        //   date:1612262884923,
        //   location: 'San Franciso, US',
        //   imageUrl: 'https://cdn.quasar.dev/img/parallax2.jpg'
        // },
        // {
        //   id:2,
        //   caption: 'Golden Gate Bridge',
        //   date:1612262884923,
        //   location: 'San Franciso, US',
        //   imageUrl: 'https://cdn.quasar.dev/img/parallax2.jpg'
        // },
        // {
        //   id:3,
        //   caption: 'Golden Gate Bridge',
        //   date:1612262884923,
        //   location: 'San Franciso, US',
        //   imageUrl: 'https://cdn.quasar.dev/img/parallax2.jpg'
        // },
        // {
        //   id:4,
        //   caption: 'Golden Gate Bridge',
        //   date:1612262884923,
        //   location: 'San Franciso, US',
        //   imageUrl: 'https://cdn.quasar.dev/img/parallax2.jpg'
        // },
      ],
      loadingPosts:false
    }
  },
  methods: {
    getPosts() {
      this.loadingPosts = true

        
      this.$axios.get('http://localhost:3000/posts').then(response => {

      this.posts = response.data
      // this.posts = []
      this.loadingPosts = false

      }).catch(err => {

        this.$q.dialog({
        title: 'Error',
        message: 'update the message'

      })

      this.loadingPosts = false
      })

    }
  },


created() {
this.getPosts()
},

  filters: {
    niceDate(value) {
      return date.formatDate(value, 'MMMM D h:mmA')
    }
  }
}
</script>

<style lang="sass">
.card-post
  .q-img
    min-height: 200px
</style>