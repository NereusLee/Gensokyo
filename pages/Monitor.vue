<script>
// import { Chart, Tooltip, Legend, Axis } from 'viser-vue'
// import('@antv/data-set/src/transform/fold')
// const DataSet = require('@antv/data-set/src/data-set')

export default {
  name: 'Monitor',
  async asyncData({ app }) {
    const chartData = await app.$axios(
      '/api/vmonitor/getViewData?mixid=6441&attrid=238000&day%5B%5D=1&day%5B%5D=0&day%5B%5D=7&from=0&end=1439'
    )
    const json = await app.$axios('/json/bins/q8rni')
    const msg = chartData.data.data
    console.log(msg)
    return { msg, json }
  },
  // async created() {
  //   let { data } = await this.$axios('https://api.myjson.com/bins/q8rni')
  //   console.log(data)
  //   this.msg = data.data
  // },
  render(h) {
    // console.log(DataSet, this.msg)
    const ds = new DataSet()
    const dv = ds.createView().source(this.msg.data)
    let names = ['当前时间', '1天前', '7天前']
    dv.transform({
      type: 'fold',
      fields: names, // 展开字段集
      key: 'date', // key字段
      value: 'value' // value字段
    })
    return <div>{this.json}</div>
    // <Chart data={this.msg.data} height={400} />
  }
}
</script>
