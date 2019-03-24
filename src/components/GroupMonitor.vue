<template>
  <div
    style="margin: 20px auto;width: 800px;height: 630px;overflow: hidden;border-radius: 3px; background-color: #dcdcdc">
    <div :style="{height:'600px',width:'800px'}" ref="myEchart">
    </div>
  </div>
</template>

<script>
  import echarts from 'echarts'
  import Global from '../../static/Global'

  export default {
    name: "monitor",

    data() {
      return {
        chart: null,
        messageId: '',
        links: [{
          source: 'user1',
          target: 'user4',
          lineStyle: {
            normal: {
              curveness: 0.1, //线段弧度
              color: 'red'
            }
          }
        }, {
          source: 'user3',
          target: 'monitor',
          lineStyle: {
            normal: {
              color: 'blue'
            }
          }
        }],
        radian: [0, 0.1, 0.2, 0.3, 0.4, 0.5]
      }
    },
    mounted() {
      this.initChart()
    },
    beforeDestroy() {
      if (!this.chart) {
        return
      }
      this.chart.dispose()
      this.chart = null
    },

    methods: {
      // =========================================================
      // websocket
      initWebSocket() { //初始化websocket
        //ws地址
        const wsuri = "ws://localhost:8080/websocket/group"
        this.websock = new WebSocket(wsuri)
        this.websock.onmessage = this.websocketonmessage
        this.websock.onclose = this.websocketclose
      },

      websocketonmessage(e) { //数据接收
        // TODO 解析后台传过来的数据
        console.log(e)
        const newLink = JSON.parse(e.data)
        alert(newLink)

        if(newLink.messageId === this.messageId){
          this.links.push({
            source: newLink.source,
            target: newLink.target,
            lineStyle: {
              normal: {
                curveness: 0, //线段弧度
                color: 'red'
              }
            }
          })
        }
        else{
          this.messageId = newLink.messageId
          this.links = [{
            source: newLink.source,
            target: newLink.target,
            lineStyle: {
              normal: {
                curveness: 0, //线段弧度
                color: 'red'
              }
            }
          }]
        }

        this.initChart()
      },

      websocketclose(e) {  //关闭
        console.log("connection closed (" + e.code + ")")
      },

      // ================================================
      // 拓扑图
      initChart() {
        this.chart = echarts.init(this.$refs.myEchart)
        // 把配置和数据放这里
        this.chart.setOption({
          title: {
            text: '网络连接',
            textAlign: 'left',
          },
          animationDurationUpdate: 1,
          animationEasingUpdate: 'quinticInOut',
          series: [
            {
              type: 'graph',
              layout: 'none',
              symbolSize: 50,
              label: {
                normal: {
                  show: true
                }
              },
              edgeSymbol: ['circle', 'arrow'],
              edgeSymbolSize: [4, 10],
              edgeLabel: {
                normal: {
                  textStyle: {
                    fontSize: 20
                  }
                }
              },
              data: [{
                name: 'user1',
                symbol: 'image://https://image-s1.oss-cn-shanghai.aliyuncs.com/software/user1.png',
                x: 300,
                y: 250,
                label: {
                  position: 'bottom'
                }
              }, {
                name: 'user2',
                symbol: 'image://https://image-s1.oss-cn-shanghai.aliyuncs.com/software/user2.png',
                x: 440,
                y: 180,
                label: {
                  position: 'bottom'
                }
              }, {
                name: 'user3',
                symbol: 'image://https://image-s1.oss-cn-shanghai.aliyuncs.com/software/user3.png',
                x: 600,
                y: 180,
                label: {
                  position: 'bottom'
                }
              }, {
                name: 'user4',
                symbol: 'image://https://image-s1.oss-cn-shanghai.aliyuncs.com/software/user4.png',
                x: 740,
                y: 250,
                label: {
                  position: 'bottom'
                }
              }, {
                name: 'user5',
                symbol: 'image://https://image-s1.oss-cn-shanghai.aliyuncs.com/software/user5.png',
                x: 780,
                y: 350,
                label: {
                  position: 'bottom'
                }
              }, {
                name: 'user6',
                symbol: 'image://https://image-s1.oss-cn-shanghai.aliyuncs.com/software/user6.png',
                x: 680,
                y: 480,
                label: {
                  position: 'bottom'
                }
              }, {
                name: 'user7',
                symbol: 'image://https://image-s1.oss-cn-shanghai.aliyuncs.com/software/user7.png',
                x: 550,
                y: 490,
                label: {
                  position: 'bottom'
                }
              }, {
                name: 'user8',
                symbol: 'image://https://image-s1.oss-cn-shanghai.aliyuncs.com/software/user8.png',
                x: 400,
                y: 450,
                label: {
                  position: 'bottom'
                }
              }, {
                name: 'user9',
                symbol: 'image://https://image-s1.oss-cn-shanghai.aliyuncs.com/software/user9.png',
                x: 300,
                y: 370,
                label: {
                  position: 'bottom'
                }
              }],

              links: this.links,
              lineStyle: {
                normal: {
                  opacity: 0.9,
                  width: 2,
                  curveness: 0,
                }
              }
            }
          ]
        })
      }
    }
  }
</script>

<style scoped>

</style>
