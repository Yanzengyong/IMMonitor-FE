<template>
  <div style="position: relative;">
    <div class="wordcloud-header">
    <img :src="headImgUrl" alt="">
      <span>{{ name }}</span>
    </div>
    <div ref="_chart" class="chart"></div>
  </div>
</template>

<style lang="scss" scoped>
  .wordcloud-header {
    width: 100%;
    height: 60px;
    position: absolute;
    padding: 10px;
    text-align: left;
    font-size: 16px;
    display: flex;
    img {
      border-radius: 5px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
    }
    span {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      margin-left: 10px;
    }
  }
</style>

<script>
import echarts from 'echarts'
import echarts_wordcloud from 'echarts-wordcloud'
let maskImage = new Image()
maskImage.src = require('@/assets/images/man.png')
import { on, off } from '@/lib/tools'

export default {
  name: 'WordCloud',
  props: {
    name: String,
    headImgUrl: String,
    chartData: Array 
  },
  data () {
    return {
      dom: null
    }
  },
  methods: {
    resize () {
      this.dom.resize()
    }
  },
  mounted () {
    this.$nextTick(() => {
      let option = {
          series: [{
            type: 'wordCloud',
    
            // The shape of the "cloud" to draw. Can be any polar equation represented as a
            // callback function, or a keyword present. Available presents are circle (default),
            // cardioid (apple or heart shape curve, the most known polar equation), diamond (
            // alias of square), triangle-forward, triangle, (alias of triangle-upright, pentagon, and star.
    
            shape: 'circle',
    
            // A silhouette image which the white area will be excluded from drawing texts.
            // The shape option will continue to apply as the shape of the cloud to grow.
    
            maskImage: maskImage,
    
            // Folllowing left/top/width/height/right/bottom are used for positioning the word cloud
            // Default to be put in the center and has 75% x 80% size.
    
            left: 'center',
            top: 'center',
            width: '90%',
            height: '90%',
            right: null,
            bottom: null,
    
            // Text size range which the value in data will be mapped to.
            // Default to have minimum 12px and maximum 60px size.
    
            sizeRange: [12, 60],
    
            // Text rotation range and step in degree. Text will be rotated randomly in range [-90, 90] by rotationStep 45
    
            rotationRange: [-90, 90],
            rotationStep: 45,
    
            // size of the grid in pixels for marking the availability of the canvas
            // the larger the grid size, the bigger the gap between words.
    
            gridSize: 8,
    
            // set to true to allow word being draw partly outside of the canvas.
            // Allow word bigger than the size of the canvas to be drawn
            drawOutOfBound: false,
    
            // Global text style
            textStyle: {
                normal: {
                    fontFamily: 'sans-serif',
                    fontWeight: 'bold',
                    // Color can be a callback function or a color string
                    color: function () {
                        // Random color
                        return 'rgb(' + [
                            Math.round(Math.random() * 160),
                            Math.round(Math.random() * 160),
                            Math.round(Math.random() * 160)
                        ].join(',') + ')';
                    }
                },
                emphasis: {
                    shadowBlur: 10,
                    shadowColor: '#333'
                }
            },
    
            // Data is an array. Each array item must have name and value property.
            // data: [{
            //     name: 'Farrah Abraham',
            //     value: 366,
            //     // Style of single text
            //     textStyle: {
            //         normal: {},
            //         emphasis: {}
            //     }
            // }]
            data: this.chartData
        }]
      };

      this.dom = echarts.init(this.$refs._chart)
      this.dom.setOption(option)
      on(window, 'resize', this.resize)
    })
  },
  beforeDestroy () {
    off(window, 'resize', this.resize)
  }
}
</script>
