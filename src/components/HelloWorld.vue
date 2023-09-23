<template>
  <div>
<!--    <a-row justify="center">-->
<!--      <a-image-->
<!--          :preview="false"-->
<!--          width="400px"-->
<!--          src="../assets/step_1.svg"/>-->
<!--    </a-row>-->
    <img alt="Vue logo" class="logo" src="../assets/step_1.svg" width="400" height="400" />
    <a-row justify="center">
      <a-col
          :span="1"
          :pull="1"
          justify="center"
      >
        <a-button @click="capture" type="primary">Capture</a-button>
      </a-col>
    </a-row>
  </div>
</template>

<script setup>
import html2canvas from 'html2canvas';
function capture() {
  console.log('capture')
  html2canvas(document.querySelector("#capture"), {
    dpi: 1000,
  }).then((itsCanvas) => {
    // document.body.appendChild(itsCanvas);
    downloadQRImg(itsCanvas, name);
  });
}
/** 根据URL下载图片 */
function downloadQRImg(canvas, name) {
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
  exportCodeConfirm();
  // }
}
function exportCodeConfirm() {
  setTimeout(() => {
    const event = new MouseEvent('click');
    /** 触发a的单击事件 */
    document.getElementById('myLink').dispatchEvent(event);
  }, 0);
}
defineProps({
  msg: {
    type: String,
    required: true
  }
})
</script>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
