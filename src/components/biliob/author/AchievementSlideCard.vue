<template>
  <BiliobSlideCard name="biliob-achievemnet" title="最近发生事件">
    <VCard
      v-for="(achievement, i) in $db.recentAchievement"
      :key="i"
      class="mx-1 px-2"
      min-width="300px"
      :to="`/author/${achievement.author.mid}`"
    >
      <VRow>
        <VCol class="pr-0" cols="auto">
          <VAvatar size="60px">
            <VImg
              :alt="achievement.author.name"
              :src="zipPic(achievement.author.face)"
            >
            </VImg>
          </VAvatar>
        </VCol>
        <VCol class="caption text-truncate">
          <div>
            <h4
              :class="`${getColor(achievement)}--text`"
              style="overflow:hidden; text-overflow: ellipsis;"
            >
              {{ achievement.author.name }} / {{ achievement.name }}
            </h4>
            <div
              class="text--secondary"
              style="overflow:hidden; text-overflow: ellipsis;"
            >
              {{ achievement.desc }}
            </div>
            <div
              class="text--disabled"
              style="overflow:hidden; text-overflow: ellipsis;"
            >
              {{ $timeFormat(achievement.date, "YYYY-MM-DD") }}
            </div>
          </div>
        </VCol>
      </VRow>
    </VCard>
  </BiliobSlideCard>
</template>
<script>
export default {
  methods: {
    getColor(achievement) {
      if (achievement.code >= 4000 && achievement.code < 5000) {
        return "green";
      }
      switch (achievement.level) {
        case 10:
          return "red";
        case 9:
          return "orange";
        case 8:
          return "purple";
        case 7:
          return "blue";
        case 6:
          return "blue";
        case 5:
          return "blue";
        case 4:
          return "blue";
        default:
          return "cyan";
      }
    }
  }
};
</script>
