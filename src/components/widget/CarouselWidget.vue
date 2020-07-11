<template>
  <swiper class="swiper" :options="swiperOption">
    <swiper-slide
    v-for="(value, i) in data"
    :key="i"
    >
        <v-card
        rounded="xl"
        :color="color(value.latest_price_change)"
        width="300px"
        height="130px"
        @click="$emit('selectTicker', value.ticker)"
        >
            <v-card-title>{{ value.ticker_name }}</v-card-title>
            <v-card-actions>
                <v-container>
                    <v-row 
                    align="end"
                    justify="end">
                        <h2>{{value.latest_price_change | persentage}}</h2>
                    </v-row>
                </v-container>
            </v-card-actions>
        </v-card>
    </swiper-slide>
    <div class="swiper-button-prev" slot="button-prev"></div>
    <div class="swiper-button-next" slot="button-next"></div>
    <!-- <div class="swiper-pagination" slot="pagination"></div> -->
  </swiper>
</template>

<script>
  import { Swiper, SwiperSlide } from 'vue-awesome-swiper'
  import 'swiper/css/swiper.css'

  export default {
    name: 'CarouselWidget',
    components: {
      Swiper,
      SwiperSlide
    },
    props: {
        data: {
            required: true,
            type: Array
        },
    },
    data() {
      return {
        swiperOption: {
          slidesPerView: 3,
          spaceBetween: 50,
          navigation: {
            nextEl: '.swiper-button-next',
            prevEl: '.swiper-button-prev'
          },
          pagination: {
            el: '.swiper-pagination',
            clickable: true
          },
          breakpoints: {
            1024: {
              slidesPerView: 3,
              spaceBetween: 40
            },
            768: {
              slidesPerView: 2,
              spaceBetween: 30
            },
            640: {
              slidesPerView: 2,
              spaceBetween: 20
            },
            320: {
              slidesPerView: 1,
              spaceBetween: 10
            }
          }
        },
      }
    },
    methods: {
        color (age) {
            if (age<0) {
                return "pink"
            }
            else if (age===0) {
                return "grey"
            } 
            else {
                return "blue"
            }
        }
    },
    filters: {
        persentage: function (value) {
            value = Math.round(((value * 100) + Number.EPSILON) * 100) / 100
            if (value<=0) {
                value = value + '%';
                return value
            } else {
                value = '+' + value + '%';
                return value
            }
        }
    },
  }
</script>

<style lang="scss" scoped>
    @import './base.scss';
    .card-outter {
        position: relative;
        padding-bottom: 50px;
    }
    .card-actions {
        position: absolute;
        bottom: 0;
    }
</style>