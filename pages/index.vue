<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="10" md="10">
      <v-card>
        <v-card-title class="headline">今日は水曜日</v-card-title>
        <v-card-text>
          <v-data-table
            :headers="headers"
            :items="ganbarus"
            :items-per-page="30"
            hide-default-footer
            class="elevation-1"
            @click:row="clickRow"
          >
          </v-data-table>
        </v-card-text>
        <v-card-actions>
          <v-spacer />
          <v-btn color="primary" nuxt to="/inspire"> Download </v-btn>
        </v-card-actions>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
export default {
  components: {},
  async asyncData({ $axios }) {
    const data = await $axios
      .$get('http://localhost:8088/ganbarus')
      .then()
      .catch()
    console.log(data)
    const ganbarus = data.ganbarus
    return { ganbarus }
  },
  data() {
    return {
      snack: false,
      snackColor: '',
      snackText: '',
      pagination: {},
      headers: [
        {
          text: '従業員番号',
          sortable: false,
          value: 'id',
        },
        { text: '名前', value: 'name' },
        { text: '退社予定時刻', value: 'exit_time' },
        { text: '在場理由', value: 'reason' },
        { text: 'がんばるか', value: 'ganbaruka' },
        { text: 'コメント', value: 'comment' },
      ],
      ganbarus: [],
      editedGanbaru: {
        id: '',
        company: '',
        name: '',
        exit_time: '',
        reason: '',
        alt_date: '',
        ganbaruka: '',
        comment: '',
      },
      select: null,
      items: ['がんばる', '超がんばる', 'がんばらない', 'がんばれない'],
    }
  },
  head() {
    return {
      title: 'Wednesday',
    }
  },
  methods: {
    clickRow(ganbaru) {
      this.$router.push('update/' + ganbaru.id)
    },
  },
}
</script>
