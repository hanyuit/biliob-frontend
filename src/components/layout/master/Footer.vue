<template>
  <VFooter class="px-0 pt-5" dark background-color="grey darken-3">
    <VLayout wrap justify-center style="text-align:center">
      <VFlex md12 lg8>
        <VLayout class="pb-5" wrap>
          <VFlex
            v-for="(eachBlock, index) in links"
            :key="index"
            lg4
            md6
            class="pl-1"
            style="text-align:left;border-left: 2px solid primary;"
          >
            <div class="title font-weight-black grey--text">
              {{ eachBlock.name }}
            </div>
            <div
              v-for="(eachLink, indexLink) in eachBlock.links"
              :key="indexLink"
            >
              <a class="caption" :href="eachLink.link">
                {{ eachLink.name }}
              </a>
            </div>
          </VFlex>
        </VLayout>
        <div>
          <span class="grey--text">但愿这个网站能够帮助到你。</span>
        </div>
        <VSpacer />
        <span class="caption">
          &copy;
          {{ new Date().getFullYear() }}
          <a href="https://www.github.com/jannchie" target="_blank"
            >Jannchie见齐</a
          >, made with
          <VIcon color="red" size="14">mdi-heart</VIcon>
          for BILIBILI
        </span>
        <div>
          <span class="grey--text caption"
            >互联网ICP备案：<a href="http://www.beian.miit.gov.cn/"
              >浙ICP备17035172号-2</a
            ></span
          >
        </div>
      </VFlex>
    </VLayout>
  </VFooter>
</template>

<script>
export default {
  data: () => {
    return {
      links: [
        {
          name: "联系站长",
          links: [
            {
              name: "见齐的魔法后花园",
              link: "https://jq.qq.com/?_wv=1027&k=593vDCo"
            },
            { name: "Jannchie@gmail.com", link: "Jannchie@gmail.com" },
            { name: "见齐的GITHUB主页", link: "https://github.com/Jannchie" },
            {
              name: "见齐B站个人空间",
              link: "https://space.bilibili.com/1850091"
            },
            { name: "见齐的技术博客", link: "http://jannchie.github.io" }
          ]
        },
        {
          name: "友情链接",
          links: [
            {
              name: "激情赞助 上海蔚文印务科技有限公司",
              link: "http://21yinshua.cn/"
            },
            {
              name: "数据合作 Kanbilibili",
              link: "https://www.kanbilibili.com/"
            },
            {
              name: "数据合作 @Damn☆You",
              link: "https://space.bilibili.com/735882"
            },
            {
              name: "数据合作 @科技号",
              link: "https://space.bilibili.com/14220823"
            },
            {
              name: "服务器提供商 阿里云",
              link:
                "https://promotion.aliyun.com/ntms/yunparter/invite.html?userCode=z82eczgc"
            },
            {
              name: "服务器提供商 腾讯云",
              link:
                "https://cloud.tencent.com/redirect.php?redirect=1005&cps_key=7db3d9f0ef080a24be84d44938108742&from=console"
            },
            {
              name: "服务器提供商 Vultr",
              link: "https://www.vultr.com/?ref=7540103"
            }
          ]
        },
        {
          name: "其他",
          links: [
            { name: "管理系统", link: "/tracer/dashboard" },
            { name: "赞助见齐", link: "https://azz.net/jannchie" },
            { name: "关于", link: "/about" },
            { name: "隐私权政策", link: "/privacy" },
            {
              name: "BiliOB的源代码",
              link: "https://github.com/Jannchie/BiliOB"
            }
          ]
        }
      ]
    };
  }
};
</script>

<style scoped>
#core-footer {
  z-index: 0;
}
.v-application a {
  color: white;
  text-decoration: none;
}
a:hover {
  color: white;
  text-decoration: underline;
}
@media (max-width: 550px) {
  .footer-items,
  .copyright {
    width: unset;
    text-align: center;
  }
}
</style>
