<template>
  <div>
    <head>
      <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
      <title>GoogleMap検索サービス</title>
    </head>
    <h3>履歴登録</h3>
    <input
      type="text"
      placeholder="検索内容を入力して「登録」を押下してください"
      v-model="set_value"
      id="set-text"
    />
    <input type="button" value="登録" @click="add" id="set-btm" />
    <h3>検索履歴一覧</h3>
    <table>
      <thead>
        <th>検索ワード</th>
        <th>検索ボタン</th>
      </thead>
      <tbody v-for="(item, index) in word_list" v-bind:key="item.id">
        <td v-text="item.word"></td>
        <td>
          <input
            type="button"
            @click="location_google_search(index)"
            value="グーグルマップ検索"
          />
        </td>
      </tbody>
    </table>
    <hr />
  </div>
</template>

<script>
export default {
  name: "Hello",
  data: () => ({
    set_value: "",
    link_title: "title",
    STORAGE_KEY: "google-map-word",
    word_list: [],
  }),
  methods: {
    fetch: function () {
      this.word_list = JSON.parse(
        localStorage.getItem(this.STORAGE_KEY) || "[]"
      );
    },
    add: function () {
      this.word_list.push({
        id: this.word_list.length,
        word: this.set_value,
      });
    },
    save: function (item) {
      localStorage.setItem(this.STORAGE_KEY, JSON.stringify(item));
    },
    location_google_search: function (index) {
      var list_index = index;
      var word = this.word_list[list_index].word;
      var url = "https://www.google.com/maps/search/" + word;
      window.open(url, "_blank");
    },
  },
  mounted: function () {
    this.fetch();
  },
  watch: {
    // オプションを使う場合はオブジェクト形式にする
    word_list: {
      // 引数はウォッチしているプロパティの変更後の値
      handler: function () {
        this.save(this.word_list);
      },
      // deep オプションでネストしているデータも監視できる
      deep: true,
    },
  },
};
</script>

<style>
body {
  background-color: lemonchiffon;
}
table,
th,
td {
  border: solid;
  border-collapse: collapse;
  margin-left: auto;
  margin-right: auto;
}
#set-btm {
  margin: 3px;
}
#set-text {
  padding: 5px;
  width: 45%;
}
</style>
