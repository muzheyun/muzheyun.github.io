<template>
  <div class="hello" id="capture" >
<!--    <div id="capture" style="padding: 10px; background: #f5da55">-->
<!--      <h4 style="color: #000; ">Hello world!</h4>-->
<!--    </div>-->
    <h1>{{ msg }}</h1>
    <button @click="capture">capture</button>
    <h2>Essential Links</h2>
    <ul>
      <li>
        <a
          href="https://vuejs.org"
          target="_blank"
        >
          Core Docs
        </a>
      </li>
      <li>
        <a
          href="https://forum.vuejs.org"
          target="_blank"
        >
          Forum
        </a>
      </li>
      <li>
        <a
          href="https://chat.vuejs.org"
          target="_blank"
        >
          Community Chat
        </a>
      </li>
      <li>
        <a
          href="https://twitter.com/vuejs"
          target="_blank"
        >
          Twitter
        </a>
      </li>
      <br>
      <li>
        <a
          href="http://vuejs-templates.github.io/webpack/"
          target="_blank"
        >
          Docs for This Template
        </a>
      </li>
    </ul>
    <h2>Ecosystem</h2>
    <ul>
      <li>
        <a
          href="http://router.vuejs.org/"
          target="_blank"
        >
          vue-router
        </a>
      </li>
      <li>
        <a
          href="http://vuex.vuejs.org/"
          target="_blank"
        >
          vuex
        </a>
      </li>
      <li>
        <a
          href="http://vue-loader.vuejs.org/"
          target="_blank"
        >
          vue-loader
        </a>
      </li>
      <li>
        <a
          href="https://github.com/vuejs/awesome-vue"
          target="_blank"
        >
          awesome-vue
        </a>
      </li>
    </ul>
  </div>
</template>

<script>
import html2canvas from 'html2canvas';
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  methods: {
    capture() {
      console.log('capture')
      html2canvas(document.querySelector("#capture")).then((itsCanvas) => {
        // document.body.appendChild(itsCanvas);
        this.downloadQRImg(itsCanvas, name);
      });
    },
    /** 根据URL下载图片 */
    downloadQRImg(canvas, name) {
      /** 新Image对象，可以理解为DOM */
      const image = new Image();
      /** 解决跨域 Canvas 污染问题 */
      // image.setAttribute('crossOrigin', 'anonymous');
      // image.onload = function() {
      /** canvas.toDataURL 返回的是一串Base64编码的URL,指定格式 PNG */
      const imgUrl = canvas.toDataURL('image/png');
      image.src = imgUrl;
      /** 生成一个a元素,并创建一个单击事件 */
      const a = document.createElement('a');
      a.download = name || 'photo'; // 设置图片名称
      a.href = imgUrl; // 将生成的URL设置为a.href属性
      a.setAttribute('id', 'myLink');
      // 开发者工具查看生成的链接href的值是否是一个完整的base64格式图片
      // console.log('标签', a);
      document.body.appendChild(a);
      this.exportCodeConfirm();
      // }
    },
    exportCodeConfirm() {
      setTimeout(() => {
        const event = new MouseEvent('click');
        /** 触发a的单击事件 */
        document.getElementById('myLink').dispatchEvent(event);
      }, 0);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.hello {
  overflow-y: visible !important ;
}
#capture {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
