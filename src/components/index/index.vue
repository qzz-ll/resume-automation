<template>
  <section id="index" class="index">
    <h3>转化为pdf工具</h3>
    <div class="wdiv">
      <label>文件名</label>
      <input class="file-name" v-model="fileName" />
    </div>
    <div class="wdiv">
      <label>小标题</label>
      <input class="" v-model="subtitle" />
    </div>
    <div class="wdiv">
      <label>内容</label>
      <textarea v-model="content"></textarea>
    </div>
    <div class="pdf-div">
      <button class="btn" @click="getPdf()">生成pdf文件</button>
    </div>
    <div id="real-content">
      <h4>{{subtitle}}</h4>
      <p>{{content}}</p>
    </div>
  </section>
</template>

<script>
/*import "@/public/stylus/basic.styl";*/
import html2canvas from "html2canvas";
import JsPDF from "jspdf";

export default {
  name: 'index',
  data(){
    return{
      subtitle:"", // 小标题
      content:"", // 简历内容
      fileName:"" // 文件名
    }
  },
  methods:{
    // 生成pdf文件
    getPdf(){
      let title = this.fileName;
      html2canvas(document.querySelector('#real-content'),{
        allowTaint:true,
        useCORS:true
      }).then(function (canvas){
        let contentWidth = canvas.width
        let contentHeight = canvas.height
        //一页pdf显示html页面生成的canvas高度;
        let pageHeight = contentWidth / 592.28 * 841.89
        //未生成pdf的html页面高度
        let leftHeight = contentHeight
        //页面偏移
        let position = 0
        //a4纸的尺寸[595.28,841.89]，html页面生成的canvas在pdf中图片的宽高
        let imgWidth = 595.28
        let imgHeight = 592.28 / contentWidth * contentHeight
        let pageData = canvas.toDataURL('image/jpeg', 1.0)
        let PDF = new JsPDF('', 'pt', 'a4')
        //有两个高度需要区分，一个是html页面的实际高度，和生成pdf的页面高度(841.89)
        //当内容未超过pdf一页显示的范围，无需分页
        if (leftHeight < pageHeight) {
          PDF.addImage(pageData, 'JPEG', 0, 0, imgWidth, imgHeight)
        } else {
          while (leftHeight > 0) {
            PDF.addImage(pageData, 'JPEG', 0, position, imgWidth, imgHeight)
            leftHeight -= pageHeight
            position -= 841.89
            //避免添加空白页
            if (leftHeight > 0) {
              PDF.addPage()
            }
          }
        }
        if(!title){
          alert("请填写文件名");
        } else {
          PDF.save(title + '.pdf')
        }
      })
    }
  }
}
</script>
<style lang="stylus" scoped>
@import "../../public/stylus/basic.styl"
  section
    max-width 750px
    margin 0 auto
    background $white
    padding 2rem 6rem 10rem
    h3
      text-align center
      font-size 3rem
      margin-bottom 3rem
    .wdiv
      margin 1rem 0
      label
        color $blue
        font-size $title
        width 15%
        float left
        text-align-last justify
        margin-right 1rem
      input
        width 80%
        height 3rem
        padding-left 1rem
      textarea
        height: 20rem
        width 80%
        padding-left 1rem
        padding-top 1rem
    .pdf-div
      text-align right
      margin 1rem 1rem 0 0
      button
        padding .2rem
    #real-content
      margin-top 2rem
      h4
        border-bottom 2px solid #333
        font-size 3rem
      p
        font-size 2.4rem
</style>
