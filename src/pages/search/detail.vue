<template>
  <view class="head">
    <search-box :keywords="keywords">
      <sort @sortClick="handleSort"></sort>
    </search-box>
  </view>
  <view style="height: 192rpx;"></view>

  // #ifndef H5
  <view class="goods">
    <view style="height: 16rpx;background-color: rgb(246, 246, 246);"></view>
    <view class="goods__item" v-for="item in itemData" :key="item.item_id" @click="goToGoodsPage(item)">
      <image mode="widthFix" lazy-load="true" :src="item.pict_url" class="goods__item__img"></image>
      // #endif
      // #ifdef H5
      <div class="goods" ref="imgDom">
        <div style="height: 16rpx;background-color: rgb(246, 246, 246);"></div>
        <div class="goods__item" v-for="item in itemData" :key="item.item_id" @click="goToGoodsPage(item)">
          <img src="http://cmsstatic.ffquan.cn//web/images/rolling.gif" :data-src="item.pict_url" alt="" class="goods__item__img">
          // #endif
          <view class="goods__item__contentContainer">
            <view class="goods__titleContainer">
              <view class="goods__titleContainer__title">
                <!--              <image mode="widthFix" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACoAAAAqCAYAAADFw8lbAAAAAXNSR0IArs4c6QAABe5JREFUWEftmHlQlVUUwH8HAZdREZfQ1CkXMELTVJzUyYVcEtPM3bBcB0SxgTST3KA0R4usBpcWsxQdlcZycpwsNTXUckRtlHBDc1fccAdRbnO98r73WIQnyzBT95837/vuPed3zj3n3PNdIcdQSgnjmoSiVLgC75zvS/q/CGkg8ZR3myzzkq5k6xN7xSrCrzrpmfEKFVDSQAXJF5HzuLgMkdiDW/RcR9CxPiuUUkMKElJa7wVSqSR+EnP4kg1Uhfn0UPfV+tKCKKweEVkqCw4Ps0DHescpRVBhBZTWPIHbeLl72oH67FJK+ZcWgDN6pJxLQ3uPHlAKP2cElNZccXFr+h8DFRdAgVIl4mTnPfrsi/DCa5BxBzYvgXNHDVirQBgcBckJkLQN9v0KGbeKDdo50Eat4e3liJhoUbevQVQ3uHkVxi9BfNub53duwLTOoH+9GkI9X6jdCDxrQ8Uq4FIO0lLhymlI2QMpiQUa5Bxoz7eQnmEOQlXsaEg7D1N+sgw49TdcPQfebRANVsBQ51Ng+dRHAjsH6u0P4XF2Hr1uPDpsDuLXMU8cdekk7N8CKbvhVLIxQHv0SR8IHIc07WR24f49iB0Fh3bmKcc5UC1CA7XtC3fvwMZvwKMWMn5JLuFKJ9XONbBmNty+nrdPdQJOXYfUaWxgLxyD6JeLAbSyJ9xNN5B61KgH736PVK5uFB3bCzcuI827mP9pFyC6O2Tczn/z+0xEugXb3qvJ7eD6pVzznfNo8HzwagBfhoG2PvQLpFlnA7V3AyyLhBkbEI9a5tm2FXB0d/6Qp5PBrwPSL9IC/XgQaINzjMKDtuiGBMcaAJ3Ni0Lh+D7oMhp07C4MgX6TkY5DC0gd67VaGwOZGUj/9yxQ7YR9vxQBVJeYoJnIU80MbOZdWBQCyduN0GYBMGbhg0RTW+Pg8lmk7yQL4OJJWG8MtY2TSeDb3hE0bgrsiC8CqF6qg7/TG9A7Ailf6WEd7Q43r0CvcKTHWNTGxbBmDtSsj7y/yQLV3v9oYG5vBwx3BI2fCb8tLSJo9vI63iaJ3Cuivn0Hdq2FgOHGq9nlpZIH0mWkBXr1PPy+0hHg0A5o0MIRdM1c2Ph1MYFqMSNiEP9eKF2ot6+GuX/Ysr+wQap+mAv375Uw6ISVSKOWqHlD4cgueLo5lHO1GD2eQEZ/Znn07BFYMc3896wDVWvCnp+hVQ9H0NUzYUtxbX2rQGTUpyhdTye2hvaDoE1vA3ErDRYE5x+jr05AuoegdHLN6PIgZByy/rtJ8OePxbD1Ovt1Y1KhsqmdX42HoR8i7fqbaqC7qQ8C8wftEIQMnmHmfj78wVHqALpwDOzfXETQmvVh4ipEb1u2ooM7YMBUpPOb5pk+Lqe/BJWqOma9NiBpK2hQ9wpmrjY0JdERNOZ10xfkGIUv+DquIuIQDauV6A5pdh8jrvUryMhPrHjU5erMIaRJ21wKVeo/4F4RqeZlGpGty5GAYdbaaQFw+fRjglavCxHLEH22PxxqcTgkPvyydnGFCSuQBi1yKbDN1zG5ZRkkrAR9ZDbvCgmrTMFv+LwxPvUERHXNU0bhPNohCPpHIq7uRqBWqts7lWUJdXWDtv2hvi+4VYD0W5B+E1KPw9FEuHgC6j5jvggqVzONSrsByIAplvHr58M6q1LYExcOVK/QHXrQLFOSVkaBbjicGZ61kVnbUJkZcC0VqtR4cLrZPK5jXfejWfeL4FH7pe0HmpNIK3RmNPaHkTFItdoWXFYW6O5e9wbb4yHrXr4SC+9RZ6AeNVeHha4aurHWSZfd2xYgv/RBH9NgR9BQ7/0Kmj6mrBJdlgPUJ0GhzDdvGRtSQepZVzqhPrEKNa6MMeoL3FS8gurYgTZ5DtRuhXIrS7AiEi0LDkc53jiH+kxXqOiyAirIX7So4i8hiZmOoFGdXEk9Mx0kUill12CWPrqIbAL3EbLgwCmt3QHUdlKM820J98LIkpagPJG85xU/vuhLA33ztor5h5aJiO16ME/Q4gcousT/QYvuQ0cJ/wIvdZODVFiJ6QAAAABJRU5ErkJggg==" class="goods__titleContainer__icon"></image>-->
                {{ item.short_title }}
              </view>
            </view>
            <view class="goods__price">
              <view class="goods__price__left">
                <text class="goods__price__text">券后</text>
                <text class="goods__price__rmb">￥</text>
                <text class="goods__price__value">{{ (item.zk_final_price - item.coupon_amount).toFixed(2) }}</text>
              </view>
              <view class="goods__price__right">
                <text class="goods__price__coupon">券</text>
                {{ item.coupon_amount }} 元
              </view>
            </view>
            <view class="goods__volume">
              <view class="goods__volume__left">
                <text class="goods__volume__text">{{ item.nick }}</text>
                &nbsp;
                <uni-icons type="right" size="24rpx" color="#888" style="transform: translateY(2rpx)"></uni-icons>
              </view>
              <text class="goods__volume__right">
                已售{{ item.volume }}
              </text>
            </view>
          </view>
          // #ifdef H5
        </div>
      </div>
      // #endif
      // #ifndef H5
    </view>
  </view>
  // #endif
  <view class="noMore" v-if="noMore">
    没有更多商品了
  </view>
</template>

<script>
import {onActivated, ref} from "vue";
import { getSearchItem } from "../../network/requests";
import {goToGoodsPage, handleDataCache, handleRequests, scrollToLowerLoad} from "../../static/common";
// #ifdef H5
import { lazyLodImg } from "../../static/common";
// #endif
import searchBox from "./search-box/search-box.vue";
import Sort from '../common/sort';
import { onLoad, onPageScroll } from '@dcloudio/uni-app';

export default {
  name: "index",
  components: {
    Sort,
    searchBox
  },
  setup(props, content) {
    let params = { material_id: 17004, q: '', has_coupon: true, sort: 'tk_total_sales_des', page_no: 1 }
    const keywords = ref('')
    onLoad((option) => {
      params.q = option.q
      keywords.value = option.q
      getItemData()
      console.log(option);
    })
    const itemData = ref([])
    const noMore = ref(false)
    const getItemData = function () {
      handleRequests(getSearchItem, params, noMore, itemData)
      params.page_no += 1
    }
    const handleSort = function (str) {
      params.sort = str
      handelCache(str)
    }
    const handelCache = handleDataCache(itemData, params.sort, getItemData, params, "page_no")

    // 下拉刷新逻辑
    scrollToLowerLoad('.goods', getItemData, 100)

    // #ifdef H5
    // 图片懒加载
    const imgDom = ref(null)
    lazyLodImg(imgDom, 400)
    onActivated(() => {
      lazyLodImg(imgDom, 400)
    })
    // #endif
    return {
      itemData,
      noMore,
      goToGoodsPage,
      handleSort,
      keywords,
      // #ifdef H5
      imgDom
      // #endif
    }
  }
}
</script>

<style scoped lang="scss">
.head {
  position: fixed;
  z-index: 300;
  width: 100%;
}
  .sort {
    display: flex;
    flex-wrap: nowrap;
    justify-content: space-around;
    align-items: center;
    font-size: 24rpx;
    color: #646566;
    height: 80rpx;
    position: fixed;
    width: 100%;
    background: #ffffff;
    z-index: 1;
    .active {
      color: rgb(254, 55, 56);
      transition: all 1s ease-out;
    }
    &__price {
      display: flex;
      justify-content: space-around;
      align-items: center;
      &__container {
        margin-left: 10rpx;
        display: flex;
        flex-direction: column;
        justify-content: space-around;
        align-items: center;
        height: 30rpx;

      }
      &__down &__downIcon{
        border-top-color: black;
      }
      &__up &__upIcon {
        border-bottom-color: black;
      }
      &__upIcon{
        border-bottom: 12rpx solid #d8d8d8;
        border-left: 10rpx solid transparent;
        border-right: 10rpx solid transparent;
        transition: all .6s ease-out;

      }
      &__downIcon{
        border-top: 12rpx solid #d8d8d8;
        border-left: 10rpx solid transparent;
        border-right: 10rpx solid transparent;
        transition: all .6s ease-out;

      }
    }
  }
  .goods {
    //height: calc(100vh - 192rpx);
    &__item {
      display: flex;
      justify-content: space-around;
      padding: 14rpx;
      border-bottom: #f5f5f5 2rpx solid;
      &__img {
        border-radius: 10px;
        width: 244rpx;
        height: 244rpx;
      }
      &__contentContainer {
        display: flex;
        flex-direction: column;

      }
    }
    &__titleContainer {
      min-height: 76rpx;
      width: 454rpx;
      flex: 1;
      &__icon {
        width: 26rpx;
        vertical-align: center;
        transform: translateY(3rpx);
      }
      &__title {
        display: inline-block;
        width: 426rpx;
        color: #333;
        font-size: 28rpx;
      }
    }
    &__price {
      font-size: 24rpx;
      font-weight: 400;
      display: flex;
      justify-content: space-between;
      align-items: center;
      &__left {

      }

      &__text {
        color: #666;
        font-size: 24rpx;

      }
      &__rmb {
        color: #ff2b22;
        font-size: 24rpx;
      }
      &__value {
        color: #ff2b22;
        font-size: 36rpx;
      }

      &__right {
        color: #ffffff;
        background: linear-gradient(
                90deg
            ,#ff8873,#ff4f4f);
        border: 1px solid red;
        border-radius: 2px;
        padding: 0 5rpx 0 0;
        line-height: 34rpx;

      }
      &__coupon {
        color: #fe3a33;
        background: #ffffff;
        padding: 0 5rpx;
        margin-right: 5rpx;
        display: inline-block;

      }
    }
    &__volume {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-top: 12rpx;

      &__left {
        background: #f4f4f4;
        font-size: 24rpx;
        color: #888;
        padding: 4rpx 10rpx;
        border-radius: 8px;
        display: flex;
        align-items: center;
      }
      &__text {
        max-width: 172rpx;
        text-overflow: ellipsis;
        white-space: nowrap;
        overflow: hidden;
      }
      &__right {
        font-size: 24rpx;
        color: #888;
      }
    }
  }
  .noMore {
    color: #969799;
    font-size: 28rpx;
    line-height: 100rpx;
    text-align: center;
    background: #f6f6f6;
  }
</style>
