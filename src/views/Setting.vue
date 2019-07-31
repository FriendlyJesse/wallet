<template>
  <div class="setting">

    <v-banner>
      <div class="banner__text">
        设置
      </div>
    </v-banner>

    <section class="select_lang">
      语言
      <el-select v-model="active" placeholder="请选择语言">
        <el-option
          v-for="item in lang"
          :key="item.code"
          :label="item.name"
          :value="item.code">
        </el-option>
      </el-select>
    </section>

  </div>
</template>

<script>
import vBanner from '../components/Banner'

export default {
  name: 'Setting',
  components: {
    vBanner
  },
  data() {
    return {
      active: 'cn',
      lang: [
        {
          code: 'cn',
          name: '中文'
        },
        {
          code: 'en',
          name: '英文'
        }
      ]
    }
  }
}
</script>

<style lang="scss" scoped>
.select_lang {
  margin-top: 50px;
  text-align: center;
  color: #30485a;
}
</style>
