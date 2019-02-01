<template>
  <div class="container">

    <div class="input-group">
      <input type="input" class="form-control" ref="inputGo">
      <span class="input-group-btn">
        <button class="btn btn-default" type="button" @click="btnGoClick">Go!</button>
      </span>
    </div>
    <paginate v-if="'last_page' in data"
      :page-count="data.last_page"
      :click-handler="functionName"
      v-model="current"
      :prev-text="'Prev'"
      :next-text="'Next'"
      :container-class="'pagination'">
    </paginate>
    <div class="row">
      <template v-for="item in images">
        <div class="col-md-4">
        <template v-for="image in item" v-key="image.id">
          <img :src="image.url" style="width: 100%; margin: 10px 5px;">
        </template>
        </div>
      </template>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      images: [],
      countPerCol: 0,
      data: {},
      current: 1
    }
  },
  mounted() {
    this.loadImage()
  },
  methods: {
    functionName(e) {
      this.loadImage(e)
    },
    loadImage(page = 1) {
      this.images = []
      fetch('https://ubuntu-uchinka.c9users.io:8081?page=' + page).then(e => {
        e.json().then(e => {
          this.data = e
          let newArr = []
          e.data = e.data.sort((a,b) => {
            return (0.5 - Math.random())
          })
          e.data.forEach((item, index) => {
            let chuck = Math.floor(index/12)
            if (!newArr[chuck]) {
              newArr[chuck] = []
            }
            newArr[chuck].push(item)
          })
        this.images = newArr
        })
      })
    },
    btnGoClick() {
      let page = + this.$refs.inputGo.value
      this.current = page
      this.loadImage(page)
    }
  }
}
</script>

<style>
  .pagination {
    list-style: none;
  }
  .pagination * {
    user-select: none;
  }
</style>
