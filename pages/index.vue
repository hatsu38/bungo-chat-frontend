<template>
  <div>
    <h2>ぶんごうチャット</h2>
    <p>ぶんごうチャットは、チャットで小説が読めるサービスです</p>
    <v-row>
      <v-col
        v-for="book in books"
        :key="book.id"
        class="d-flex child-flex"
        cols="6"
      >
        <v-card flat tile class="d-flex">
          <v-img
            :src=book.rakuten_book_info.medium_image_url
            lazy-src="https://picsum.photos/id/11/100/60`"
            aspect-ratio="1"
            class="grey lighten-2"
          >
            <template v-slot:placeholder>
              <v-row
                class="fill-height ma-0"
                align="center"
                justify="center"
              >
                <v-progress-circular indeterminate color="grey lighten-5"></v-progress-circular>
              </v-row>
            </template>
          </v-img>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>
import axios from "axios"
export default {
  data: () => ({
    books: []
  }),
  async created() {
    const res = await axios.get(
      'https://bungo-chat-api.herokuapp.com/api/ranking'
    )
    this.books = res.data.books
  }
}
</script>
