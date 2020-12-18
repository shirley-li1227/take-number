<template>
  <van-row type="flex" justify="space-between" align="center">
    <van-col :span="12">
      <van-image
        @click="onPreview"
        round
        width="10rem"
        height="10rem"
        fit="cover"
        :src="img"
      />
    </van-col>
    <van-col :span="8">
      <van-button type="primary" @click="toForm">表单</van-button>
    </van-col>
  </van-row>
</template>
<script>
import { Image, ImagePreview, Button, Col, Row } from "vant";
export default {
  components: {
    [Image.name]: Image,
    [Button.name]: Button,
    [Col.name]: Col,
    [Row.name]: Row,
  },
  data() {
    return {
      img: 'https://img.yzcdn.cn/vant/cat.jpeg'
    }
  },
  methods: {
    onPreview() {
      ImagePreview([this.img]);
    },
    toForm() {
      this.$router.push('form');
    },
  }
}
</script>
<style lang="less" scoped>
</style>