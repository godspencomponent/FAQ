<template>
  <div class="component">
    <div class="list">
      <template>
        <div v-if='type == 1'>
          <div class="faq" :class="[v.isSender ? 'is-sender' : 'not-sender']" v-for='(v, i) in list' :key='i'>
            <div class="faq-content" v-if='v.isSender'>
              <div v-html='v.content'></div>
            </div>
            <div class="faq-user">
              <img :src="v.img" alt="">
              <span class="faq-user-nick">{{v.nick}}</span>
            </div>
            <div class="faq-content" v-if='!v.isSender'>
              <div v-html='v.content'></div>
            </div>
          </div>
        </div>
        <div v-else>
          <div class="faq2" v-for='(v, i) in list' :key='i'>
            <section>
              <div class="headPortrait">
                <img :src="v.img" alt="">
              </div>
              <div class="faq2-content">
                <h1 class="fd-title" :style="{color: v.titleColor, fontSize: v.titleFont + 'px'}">{{v.name}}</h1>
                <div class="faq2-body">
                  <div v-html='v.content'></div>
                </div>
              </div>
            </section>
          </div>
        </div>
      </template> 
    </div>
  </div>
</template>

<script>
  import {VueExtend} from 'godspen-lib'

  export default {
    mixins: [VueExtend.mixin],
    name: 'FAQ',
    label: process.env.LABEL,
    style: process.env.STYLE,
    props: {
      type: {
        type: Number,
        default: 1,
        editor: {
          ignore: true
        }
      },
      list: {
        type: Array,
        default () {
          return [
            {
              name: '问',
              isSender: true,
              img: 'https://ymm-maliang.oss-cn-hangzhou.aliyuncs.com/ymm-maliang/resource/ymm_1564561942960.jpg',
              nick: '靓女',
              content: '码良有哪些能力？'
            },
            {
              name: '答',
              isSender: false,
              img: 'https://ymm-maliang.oss-cn-hangzhou.aliyuncs.com/ymm-maliang/resource/ymm_1564568927763.png',
              nick: '帅磊',
              content: '可以快速制作H5页面。无需掌握复杂的编程技术，通过简单拖拽、少量配置即可制作精美的页面，可用于营销场景下的页面制作。'
            }
          ]
        },
        editer: {
          ignore: true // 忽略，码良基础在编辑器中不显示
        }
      }
    },
    computed: {
    },
    editorMethods: {
    },
    methods: {
      delItem (i) {
        this.componentInfo.list.splice(i, 1)
      },
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus" type="text/stylus" scoped>
  .component {
    width 100%
    .faq{
      display: flex;
      min-width: 60px;
      padding: 8px 12px;
      background-color: transparent;
    }
    .faq2 {
      padding 8px 15px 8px 12px
      section {
        position relative
        .headPortrait {
          position: absolute;
          top: 0;
          left: 0;
          img {
            width 30px
            height 30px
          }
        }
        .faq2-content {
          padding-left 40px
          .fd-title {
            font-size: 16px;
            color: #333;
          }
          .faq2-body {
            margin-top: 10px;
            font-size: 14px;
            color: #333;
          }
        }
      }
    }
    .is-sender {
      justify-content: flex-end;
      .faq-content {
        color: #fff;
        border: 1px solid #4947bb;
        background-color: #6260e1;
        &:before {
          content ''
          right: -5.5px;
          background: #6260e1;
          border: 1px solid #4947bb;
          border-bottom: 0;
          border-left: 0;
        }
      }
      .faq-user {
        margin-left 12px
      }
    }
    .not-sender  {
      justify-content: start;
      .faq-user {
        margin-right 12px
      }
      .faq-content {
        border: 1px solid #d9d9d9;
        color: #333;
        background-color: #fff;
        &:before {
          content ''
          left: -5.5px;
          background: #fff;
          border: 1px solid #d9d9d9;
          border-top: 0;
          border-right: 0;
        }
      }
    }
    .faq-content {
      position: relative;
      max-width: 220px;
      height: fit-content;
      padding: 8px 10px;
      box-sizing: border-box;
      border-radius: 8px;
      font-size: 14px;
      &:before {
        content ''
        position: absolute;
        top: 14px;
        width: 10px;
        height: 10px;
        -webkit-transform: rotate(45deg);
        -ms-transform: rotate(45deg);
        transform: rotate(45deg);
        box-sizing: border-box;
      }
    }
    .faq-user {
      display: flex;
      -webkit-box-orient: vertical;
      -webkit-box-direction: normal;
      flex-direction: column;
      -webkit-box-align: center;
      align-items: center;
      width: 48px;
      height: 65px;
      img {
        width: 40px;
        height: 40px;
        margin-bottom: 8px;
        border-radius: 50%;
        box-sizing: border-box;
      }
      .faq-user-nick {
        width:48px;
        text-align: center;
        font-size: 12px;
        color: #999;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
      }
    }
  }
</style>
