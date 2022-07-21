<script setup>
import { ref } from 'vue'

const ubikeStops = ref([])

fetch('https://tcgbusfs.blob.core.windows.net/blobyoubike/YouBikeTP.gz')
  .then((res) => res.json())
  .then((res) => {
    // 將 json 轉陣列後存入 this.ubikeStops
    ubikeStops.value = Object.keys(res.retVal).map((key) => res.retVal[key])
  })

const timeFormat = (t) => {
  var date = [],
    time = []

  date.push(t.substr(0, 4))
  date.push(t.substr(4, 2))
  date.push(t.substr(6, 2))
  time.push(t.substr(8, 2))
  time.push(t.substr(10, 2))
  time.push(t.substr(12, 2))

  return date.join('/') + ' ' + time.join(':')
}
</script>

<template>
  <h1>YouBike 臺北市公共自行車即時資訊</h1>

  <div id="app">
    <label> 站點名稱搜尋: <input type="text" /> </label>

    <table class="table table-striped">
      <thead>
        <tr>
          <th>#</th>
          <th>場站名稱</th>
          <th>場站區域</th>
          <th>目前可用車輛</th>
          <th>總停車格</th>
          <th>資料更新時間</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="s in ubikeStops">
          <td>{{ s.sno }}</td>
          <td>{{ s.sna }}</td>
          <td>{{ s.sarea }}</td>
          <td>{{ s.sbi }}</td>
          <td>{{ s.tot }}</td>
          <td>{{ timeFormat(s.mday) }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
