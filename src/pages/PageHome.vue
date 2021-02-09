<template>
  <q-page class="constrain q-pa-md">
    <div class="row q-col-gutter-lg">
      <div class="col-12">
        <q-pull-to-refresh @refresh="refresh">
          <template v-if="!loadingPosts && posts.length">
            <q-infinite-scroll @load="onLoad" :offset="250">
            <q-card v-ripple v-for="(post, index) in posts" :key="index" class="card-post q-mb-md q-hoverable" flat bordered
              @click.native="onClick(post.url)">
              <q-item>
                <q-item-section avatar>
                  <q-avatar>
                    <img src="https://cdn.quasar.dev/img/boy-avatar.png" />
                  </q-avatar>
                </q-item-section>

                <q-item-section>
                  <q-item-label class="text-bold">{{ post.title }}</q-item-label>
                  <q-item-label caption>
                    {{ post.blog_entity.name }}
                  </q-item-label>
                </q-item-section>
              </q-item>

              <q-separator />

              <q-img :src="post.featured_image_url" />

              <q-card-section>
                <div>{{ post.description }}</div>
                <div class="text-caption text-grey">
                  {{ post.date | niceDate }}
                </div>
              </q-card-section>
            </q-card>
            <template v-slot:loading>
              <div class="row justify-center q-my-md">
                <q-spinner-dots color="primary" size="40px" />
              </div>
            </template>
            </q-infinite-scroll>
          </template>

          <template v-else-if="!loadingPosts && !posts.length">
            <h5 class="text-center text-grey">No posts yet.</h5>
          </template>

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
        </q-pull-to-refresh>
      </div>
    </div>
  </q-page>
</template>

<script>
  import { date } from 'quasar'
  import { openURL } from 'quasar'


  export default {
    name: "PageHome",
    data() {
      return {
        posts: [],
        loadingPosts: false,
        page: 1
      };
    },
    methods: {
      onClick: function (link) {
        openURL(link)
      },
      getPosts() {
        this.loadingPosts = true;
        this.$axios.get('https://agile-springs-66116.herokuapp.com/api/v1/cities/615/posts.json')
          .then(response => {
            this.posts = response.data;
            this.loadingPosts = false;
          })
          .catch(err => {
            this.$q -
              dialog({
                title: "Error",
                message: "Pics not found"
              });
            this.loadingPosts = false;
          });
      },
      onLoad (index, done) {
        setTimeout(() => {
          if (this.posts) {
            this.page = this.page+1
            this.$axios.get(`https://agile-springs-66116.herokuapp.com/api/v1/cities/615/posts.json?page=${this.page}`)
            .then(response => {
              this.posts.push(...response.data)
              this.loadingPosts = false;
              done()
            })
            .catch(err => {
              this.$q -
                dialog({
                  title: "Error",
                  message: "Pics not found"
                });
              this.loadingPosts = false;
            });
          }
        }, 2000)
      },
      refresh (done) {
        setTimeout(() => {
          this.posts = []
          this.getPosts()
          done()
        }, 1000)
      }
    },
    filters: {
      niceDate(value) {
        return date.formatDate(value, "MMMM D h:mmA");
      }
    },
    created() {
      this.getPosts();
    }
  };

</script>

<style lang="sass">
  .card-post
    .q-img
      min-height: 200px

</style>
