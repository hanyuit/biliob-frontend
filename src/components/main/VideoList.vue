<template>
  <div class="video-list-main">
    <BiliobCard class="card-tabs">
      <div>
        <VSearchForm
          slot="search"
          hint="请输入标题、分区或者av号"
          @getSearchValue="getSearchValue"
        />
      </div>
      <VLayout slot="header" row>
        <VFlex style="width: 70%" grow>
          <div>
            <VTabs color="transparent" slider-color="white">
              <VIcon right>mdi-sort</VIcon>
              <VTab @click="sortChange(0)">
                <VIcon>
                  mdi-play-circle-outline
                </VIcon>
                <span v-if="$vuetify.breakpoint.mdAndUp">
                  播放
                </span>
              </VTab>
              <VTab @click="sortChange(1)">
                <VIcon>
                  mdi-thumb-up-outline
                </VIcon>
                <span v-if="$vuetify.breakpoint.mdAndUp">
                  点赞
                </span>
              </VTab>
              <VTab @click="sortChange(2)">
                <VIcon>
                  mdi-coin
                </VIcon>
                <span v-if="$vuetify.breakpoint.mdAndUp">
                  硬币
                </span>
              </VTab>
              <VTab @click="sortChange(3)">
                <VIcon>
                  mdi-message-bulleted
                </VIcon>
                <span v-if="$vuetify.breakpoint.mdAndUp">
                  弹幕
                </span>
              </VTab>
              <VTab @click="sortChange(4)">
                <VIcon>
                  mdi-folder-star
                </VIcon>
                <span v-if="$vuetify.breakpoint.mdAndUp">
                  收藏
                </span>
              </VTab>
              <VTab @click="sortChange(5)">
                <VIcon hidden-md-and-down>
                  mdi-share
                </VIcon>
                <span v-if="$vuetify.breakpoint.mdAndUp">
                  分享
                </span>
              </VTab>
            </VTabs>
          </div>
        </VFlex>
        <VDivider vertical style="margin: 2px 2px !important" />
        <VFlex style="width:30%" align-content-end>
          <VTabs right color="transparent" slider-color="white">
            <VIcon left>mdi-calendar-blank-outline</VIcon>
            <VTab @click="daysChange(1)">
              <VIcon>
                mdi-numeric-1-box
              </VIcon>
            </VTab>
            <VTab @click="daysChange(3)">
              <VIcon>
                mdi-numeric-3-box
              </VIcon>
            </VTab>
            <VTab @click="daysChange(31)">
              <VIcon>
                mdi-alpha-a-box
              </VIcon>
            </VTab>
          </VTabs>
        </VFlex>
      </VLayout>
      <VSlideYTransition group>
        <div
          v-for="eachVideo in videoList.content"
          :key="eachVideo.aid"
          class="video-cards"
          @click.stop="toVideoDetail(eachVideo.mid, eachVideo.aid)"
        >
          <div style="padding:5px;display:flex">
            <div>
              <VImg
                class="video-img"
                :src="zipPic(eachVideo.pic.replace('http:', ''))"
                :lazy-src="zipPic(eachVideo.pic.replace('http:', ''))"
              />
            </div>
            <div style="margin-left:10px;overflow:hidden">
              <div
                class="font-weight-bold video-title text-no-wrap text-truncate"
              >
                {{ eachVideo.title }}
              </div>
              <div class="caption subtext video-info">
                <VIcon small> mdi-account-box-outline </VIcon
                >{{ eachVideo.authorName }}
                <VIcon small> mdi-bookmark-outline </VIcon
                >{{ eachVideo.channel }}
              </div>
              <VRow no-gutters>
                <VSpacer></VSpacer>
                <VCol cols="auto">
                  <ObserveStatus class="observe-status" :object="eachVideo" />
                </VCol>
              </VRow>
            </div>
          </div>
          <VDivider></VDivider>
        </div>
      </VSlideYTransition>
      <VBtn
        v-if="!notFound"
        block
        outlined
        style="border-width:1px"
        color="primary"
        :disabled="nextBtnDisabled"
        tile
        @click.stop="next"
        >{{ nextBtnText }}</VBtn
      >
      <div v-else>
        <h4 class="primary--text">
          <VIcon class="primary--text">mdi-ship-wheel</VIcon
          >抱歉！什么都没有找到QwQ
        </h4>
        <p>
          搜索功能可能并不完善，为了精确搜索请在上方输入相关视频的AV号！
        </p>
        <p>
          如果搜索ID仍然没有结果，可能是因为该UP主并未被本站观测。你可以点击页面右下角的圆形按钮进行添加！
        </p>
      </div>
    </BiliobCard>
  </div>
</template>

<script>
import VSearchForm from "../common/VSearchForm.vue";
import ObserveStatus from "../common/ObserveStatus.vue";
export default {
  name: "VideoList",
  components: {
    VSearchForm,
    ObserveStatus
  },
  data() {
    return {
      sort: 0,
      videoList: [],
      currentApiurl: "/video",
      currentPage: 0,
      text: String(),
      nextBtnText: "请给我更多...",
      nextBtnDisabled: false,
      notFound: false,
      days: 1
    };
  },
  watch: {
    text: function() {
      this.currentPage = 0;
      this.axios
        .get(
          `${this.currentApiurl}?page=${this.currentPage}&pagesize=20&text=${this.text}&sort=${this.sort}&days=${this.days}`
        )
        .then(response => {
          this.videoList.content = response.data.content;
          if (this.videoList.content.length == 0) {
            this.notFound = true;
          } else {
            this.notFound = false;
          }
        });
    },
    currentPage: function changePage(page) {
      if (this.page >= 20) {
        this.nextBtnText = "不能显示更多了";
        this.nextBtnDisabled = true;
        return;
      }
      this.axios
        .get(
          `${this.currentApiurl}?page=${page}&pagesize=20&text=${this.text}&sort=${this.sort}&days=${this.days}`
        )
        .then(response => {
          // 判断是否为最后一页
          if (response.data.last) {
            this.nextBtnText = "没有更多了";
            this.nextBtnDisabled = true;
          }

          response.data.content.forEach(e => {
            this.videoList.content.push(e);
          });
        });
    }
  },
  created() {
    this.axios
      .get(
        `${this.currentApiurl}?page=${this.currentPage}&pagesize=20&text=${this.text}&sort=${this.sort}&days=${this.days}`
      )
      .then(response => {
        this.refreshList(response);
      });
  },
  methods: {
    refreshList(response) {
      this.videoList = response.data;
      // 判断是否为最后一页
      if (response.data.last) {
        this.nextBtnText = "没有更多了";
        this.nextBtnDisabled = true;
      }
    },
    onScroll() {
      var scrollTop =
        document.documentElement.scrollTop || document.body.scrollTop;
      var windowHeight =
        document.documentElement.clientHeight || document.body.clientHeight;
      var scrollHeight =
        document.documentElement.scrollHeight || document.body.scrollHeight;
      if (scrollTop + windowHeight == scrollHeight) {
        // this.currentPage += 1;
      }
    },
    next() {
      this.currentPage += 1;
    },
    getSearchValue(value) {
      this.text = value;
    },
    handleChoosed(index, row) {
      this.$router.push({
        path: "/author/" + row.mid + "/video/" + row.aid
      });
    },
    sortChange(sort) {
      this.sort = sort;
      this.currentPage = 0;
      this.axios
        .get(
          `${this.currentApiurl}?page=${this.currentPage}&pagesize=20&text=${this.text}&sort=${this.sort}&days=${this.days}`
        )
        .then(response => {
          this.refreshList(response);
        });
    },
    daysChange(days) {
      this.days = days;
      this.currentPage = 0;
      this.axios
        .get(
          `${this.currentApiurl}?page=${this.currentPage}&pagesize=20&text=${this.text}&sort=${this.sort}&days=${this.days}`
        )
        .then(response => {
          this.refreshList(response);
        });
    },
    toVideoDetail(mid, aid) {
      this.$router.push("/author/" + mid + "/video/" + aid);
    }
  }
};
</script>

<style scoped>
.face {
  position: relative;
  height: 60%;
  width: 60%;
  align-content: center;
  border-radius: 4px;
}

.el-table td {
  padding: 2px 0;
}

.video-cards {
  margin: 10px 0px;
  padding: 0px 0px;
  border-radius: 5px;
  position: relative;
}

.video-title {
  font-size: 15px;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

.observe-status {
  position: absolute;
  bottom: 5px;
  right: 5px;
}

.video-info {
  margin-top: 5px;
}
@media only screen and (max-width: 1029px) {
  .video-img {
    border-radius: 5px;
    width: 112px;
    height: 70px;
  }
}
@media only screen and (min-width: 1029px) {
  .video-img {
    border-radius: 5px;
    width: 120px;
    height: 75px;
  }
}
</style>
