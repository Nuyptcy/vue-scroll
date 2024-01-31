<template>
  <div class="scroll-container" ref="scrollContainer">
    <div class="content" v-for="item in data" :key="item.id">
      <div class="card">
        <div class="span-style">
          <span class="title">編號：</span>
          <span> {{ item.id }}</span>
        </div>
        <div class="span-style">
          <span class="title">標題：</span><span> {{ item.title }}</span>
        </div>
        <div class="span-style">
          <span>分類：</span><span>{{ item.category }}</span>
        </div>
        <div class="span-style">
          <span>價格：</span><span>{{ item.price }}</span>
        </div>
        <div class="span-style">
          <span>介紹：</span><span>{{ item.description }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue'

const data = ref([])
const scrollContainer = ref(null)

const getMockData = async () => {
  const res = await fetch('https://fakestoreapi.com/products')
  const jsonRes = await res.json()
  return jsonRes
}

const loadMoreData = async () => {
  const newData = await getMockData()
  data.value = [...data.value, ...newData]
}

onMounted(async () => {
  const list = await getMockData()
  data.value = list
  scrollContainer.value.addEventListener('scroll', () => {
    if (
      scrollContainer.value.scrollTop + scrollContainer.value.clientHeight >=
      scrollContainer.value.scrollHeight
    ) {
      loadMoreData()
    }
  })
})
</script>

<style scoped>
.span-style {
  display: flex;
}
.span-style span:first-child {
  width: 100px;
  height: 20px;
}
.span-style span:last-child {
  max-width: 500px;
}
.scroll-container {
  width: 900px;
  height: 600px;
  display: flex;
  flex-direction: column;
  overflow: scroll;
  margin: 30px;
  padding: 20px;
  border: 1px solid darkgrey;
  box-shadow: 0px 2px 10px 1px darkgray;
}
.content {
  margin-bottom: 30px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.card {
  width: 650px;
  border: 1px dotted darkgrey;
}
</style>
