<template>
  <div class="flex-page">
    <div ref="postHtml" class="flex-page-content">
      <div class="primary-text">
        <primary-title :offset-bottom="-0.2" title="会诊总结"></primary-title>
        <div v-if="summary.length > 0" class="save-img"><span class="save-page" @click="createPoster()"></span></div>
      </div>
      <div class="summary-box">
        <template v-if="visitType === 'preVisited'">
          <div class="summary-title">肿瘤类型</div>
          <div class="summary-info">{{ preSummary.tumorTypeName || '无' }}</div>
          <div class="summary-title">临床分期</div>
          <div class="summary-info">{{ preSummary.clinicalStages || '无' }}</div>
          <div class="summary-title">本次主诉</div>
          <div class="summary-info">{{ preSummary.chiefComplaint || '无' }}</div>
          <div class="summary-title">主任建议</div>
          <div class="summary-info">{{ preSummary.suggestion || '无' }}</div>
          <div class="summary-title">建议解读</div>
          <div class="summary-info">{{ preSummary.suggestionDetail || '无' }}</div>
        </template>
        <template v-else-if="summary.length > 0">
          <div class="summary-desc">结合各专家意见如下</div>
          <div class="summary-content">{{ summary[0].reasonName }}</div>
          <div class="summary-img-box">
            <template v-for="(media, i) in summary[0].mediaList">
              <van-image v-if="media.mediaCategoryId === IMG_CATEGORY_ID" :key="i" :src="media.mediaUrl" :radius="6" class="summary-img" />
            </template>
          </div>
          <template v-for="(media, i) in summary[0].mediaList">
            <a v-if="media.mediaCategoryId === PDF_CATEGORY_ID" :key="i" :href="media.mediaUrl" class="summary-pdf">会诊总结.pdf</a>
          </template>
        </template>
      </div>
      <div class="mark-name">
        <img v-if="summary[0]?.isSignatured && summary[0]?.providerSignatureUrl" :src="summary[0]?.providerSignatureUrl" alt="" />
        <span v-else>{{ summary[0]?.providerName }}</span>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, Ref, ref, onMounted, nextTick } from 'vue';
import html2canvas from 'html2canvas';
export default defineComponent({
  components: {
  },
  props: {
    visitId: {
      type: String,
      required: true,
    },
    visitType: {
      type: String,
      required: false,
      default: 'preVisited',
    },
  },
  setup: (props) => {
    const postHtml = ref();
    const preSummary = {}
    const summary = [];
    // 获取会诊总结
    const getConsulationSummary = () => {};
    // 图片转为base64编码
    function image2Base64(img: HTMLImageElement) {
      // eslint-disable-next-line prefer-const, no-var
      let canvas = document.createElement('canvas'); // 一定要设置为let，不然图片不显示
      canvas.width = img.width;
      canvas.height = img.height;
      const ctx = canvas.getContext('2d');
      ctx!.drawImage(img, 0, 0, img.width, img.height);
      const dataURL = canvas.toDataURL('image/png');
      return dataURL;
    }
    /** 导出/下载 二维码 */
    const exportCodeConfirm = () => {
      setTimeout(() => {
        const event = new MouseEvent('click');
        /** 触发a的单击事件 */
        document.getElementById('myLink')!.dispatchEvent(event);
      }, 0);
    };
    /** 根据URL下载图片 */
    const downloadQRImg = (canvas: any, name: string) => {
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
    };
    /** 将html元素转换成海报图片 */
    const createPoster = async () => {
      const name = `会诊总结`;
      const posterHtml = postHtml.value;
      // }, 1000);
      // 生成图片
      const domObj = posterHtml;
      await nextTick();
      // 确保页面渲染完毕
      setTimeout(() => {
        html2canvas(domObj, {
          allowTaint: true,
          imageTimeout: 500,
          useCORS: true,
          scrollY: -18,
          scrollX: -0,
          height: domObj.clientHeight,
        }).then((itsCanvas) => {
          // document.body.appendChild(itsCanvas);
          downloadQRImg(itsCanvas, name);
        });
      }, 1000);
    };
    onMounted(async () => {
      await getConsulationSummary();
      // 默认执行，加载时间较长不放在点击事件中，达到正常保存页面中的在线图片链接的效果
      nextTick(() => {
        setTimeout(() => {
          const imgTag = document.querySelectorAll('img');
          // 获取base64编码
          let base64 = '';
          // 将页面的img的src值改为base64格式
          imgTag.forEach((item, i) => {
            const img = new Image();
            img.setAttribute('crossOrigin', 'anonymous');
            // 图片链接地址后增加随机数防止缓存
            img.src = `${imgTag[i].src}&v=${Math.random()}`;
            img.onload = () => {
              base64 = image2Base64(img);
              imgTag[i].setAttribute('src', base64);
            };
          });
        }, 3000);
      });
    });
    return {
      summary,
      preSummary,
      IMG_CATEGORY_ID: '301200001',
      PDF_CATEGORY_ID: '301200004',
      postHtml,
      createPoster,
    };
  },
});
</script>
<style lang="scss" scoped>
.summary-box {
  padding: 0 2rem 2rem 2rem;
}
.summary-title {
  font-size: 1.5rem;
  font-weight: 400;
  color: $icon-color;
  margin-bottom: 1.4rem;
}
.summary-info {
  background: $bg-text-color;
  border-radius: 0.4rem;
  padding: 1rem;
  color: $grey-color;
  font-size: 1.4rem;
  font-weight: 400;
  margin-bottom: 1.6rem;
}
.summary-desc {
  color: $arrow-color;
  font-size: 1.4rem;
  font-weight: 400;
  margin: 0 0 1.2rem 0.8rem;
}
.summary-content {
  margin: 0 0 2rem 1rem;
  color: $text-color;
  font-size: 1.4rem;
  font-weight: 400;
  li {
    line-height: 3rem;
    list-style: none;
  }
}
.summary-img-box {
  display: flex;
  flex-wrap: wrap;
  padding-bottom: 1rem;
  .summary-img {
    width: 7.4rem;
    height: 7.4rem;
    margin-right: 0.8rem;
  }
}
.summary-pdf {
  font-size: 1.5rem;
  font-weight: 400;
  color: var(--font-color-primary);
  margin-left: 0.4rem;
}
.save-img {
  height: 4.5rem;
  display: flex;
  justify-content: flex-end;
  align-items: center;
  color: $arrow-color;
  span {
    display: inline-block;
    width: 11.9rem;
    height: 4.5rem;
    background-size: 100%;
  }
}
.flex-page-content {
  overflow-y: visible !important ;
}
.flex-page {
  height: auto;
}
.mark-name {
  padding: 0 2rem 0 2rem;
  img {
    width: 9rem;
    height: 6rem;
  }
  span {
    font-size: 1.7rem;
    font-family: PingFang SC-Bold, PingFang SC;
    font-weight: 400;
    color: #2b2b2b;
    line-height: 3rem;
  }
}
.primary-text {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1.5rem 0 1rem 2rem;
}
</style>
