<template>
  <v-row justify="center" align="center">
    <v-col cols="8">
      <v-card>
        <v-card-title>{{ id }} 更新画面</v-card-title>
        <v-divider></v-divider>
        <v-card-text>
          <v-row>
            <v-col cols="4">
              <v-text-field
                v-model="ganbaru.id"
                label="従業員番号"
                prepend-icon="mdi-account"
                readonly
                outlined
                dense
              ></v-text-field>
            </v-col>
            <v-col cols="4">
              <v-text-field
                v-model="ganbaru.name"
                label="名前"
                readonly
                outlined
                dense
              ></v-text-field>
            </v-col>
            <v-col cols="6">
              <v-text-field
                v-model="ganbaru.company"
                prepend-icon="mdi-city"
                label="会社名"
                readonly
                outlined
                dense
              ></v-text-field> </v-col
          ></v-row>
          <v-row>
            <v-col cols="4">
              <v-menu
                ref="menu"
                v-model="menu2"
                :close-on-content-click="false"
                :nudge-right="40"
                :return-value.sync="ganbaru.exit_time"
                transition="scale-transition"
                offset-y
                max-width="290px"
                min-width="290px"
              >
                <template #activator="{ on, attrs }">
                  <v-text-field
                    v-model="ganbaru.exit_time"
                    label="Picker in menu"
                    prepend-icon="mdi-clock-time-four-outline"
                    readonly
                    outlined
                    v-bind="attrs"
                    v-on="on"
                  ></v-text-field>
                </template>
                <v-time-picker
                  v-if="menu2"
                  v-model="ganbaru.exit_time"
                  full-width
                  @click:minute="$refs.menu.save(ganbaru.exit_time)"
                ></v-time-picker>
              </v-menu>
            </v-col>
            <v-col cols="8"
              ><v-text-field
                v-model="ganbaru.reason"
                label="在場理由"
                outlined
                counter="20"
                :rules="[rules.length(20)]"
              ></v-text-field
            ></v-col>
            <v-col cols="4">
              <v-menu
                v-model="menu"
                :close-on-content-click="false"
                :nudge-right="40"
                transition="scale-transition"
                offset-y
                min-width="auto"
              >
                <template #activator="{ on, attrs }">
                  <v-text-field
                    v-model="ganbaru.alt_date"
                    label="振替定時退場日"
                    prepend-icon="mdi-calendar"
                    readonly
                    outlined
                    v-bind="attrs"
                    v-on="on"
                  ></v-text-field>
                </template>
                <v-date-picker
                  v-model="ganbaru.alt_date"
                  no-title
                  @input="menu = false"
                ></v-date-picker>
              </v-menu>
            </v-col>
          </v-row>
          <v-divider></v-divider>
          <v-radio-group v-model="ganbaru.ganbaruka" row>
            <v-radio label="がんばる" value="がんばる"></v-radio>
            <v-radio label="超がんばる" value="超がんばる"></v-radio>
            <v-radio label="がんばらない" value="がんばらない"></v-radio>
            <v-radio label="がんばれない" value="がんばれない"></v-radio>
          </v-radio-group>

          <v-textarea
            v-model="ganbaru.comment"
            label="コメント"
            outlined
            counter="50"
            :rules="[rules.length(50)]"
          ></v-textarea>
        </v-card-text>
        <v-card-actions>
          <v-btn text color="error" @click="clickCancel">CANCEL</v-btn>
          <v-spacer></v-spacer>
          <v-btn text color="success" @click="clickUpdate">UPDATE</v-btn>
        </v-card-actions>
      </v-card>
    </v-col>
  </v-row>
</template>
<!--{-->
<!--text: ,-->
<!--sortable: false,-->
<!--value: 'id',-->
<!--},-->
<!--{ text: '', value: 'name' },-->
<!--{ text: '', value: 'exit_time' },-->
<!--{ text: '', value: 'reason' },-->
<!--{ text: '', value: 'ganbaruka' },-->
<!--{ text: '', value: 'comment' },-->
<script>
export default {
  async asyncData({ $axios, params }) {
    const data = await $axios
      .$get('http://localhost:8088/ganbarus/' + params.id)
      .then()
      .catch()
    const ganbaru = data.ganbaru
    return { ganbaru }
  },
  data() {
    return {
      ganbaru: {
        id: '11',
        company: '',
        name: '',
        exit_time: '',
        reason: '',
        alt_date: '',
        ganbaruka: '',
        comment: '',
      },
      rules: {
        length: (len) => (v) =>
          (v || '').length <= len ||
          `Invalid character length, required ${len}`,
        row: null,
      },
      menu: false,
      modal: false,
      menu2: false,
    }
  },
  computed: {
    id() {
      return this.$route.params.id
    },
  },
  methods: {
    clickCancel() {
      this.$router.push('/')
    },
    clickUpdate() {
      const params = new URLSearchParams()
      params.append('exit_time', this.ganbaru.exit_time)
      params.append('reason', this.ganbaru.reason)
      params.append('alt_date', this.ganbaru.alt_date)
      params.append('ganbaruka', this.ganbaru.ganbaruka)
      params.append('comment', this.ganbaru.comment)
      this.$axios
        .$put('http://localhost:8088/ganbarus/' + this.id, params)
        .then()
        .catch()
      this.$router.push('/')
    },
  },
}
</script>

<style scoped></style>
