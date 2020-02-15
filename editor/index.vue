<template>
  <div class="component-editor">
    <div>
      <span>样式：</span>
      <div class='list-type' @click='chooseType(1)'>
        <img src="https://ymm-maliang.oss-cn-hangzhou.aliyuncs.com/ymm-maliang/resource/ymm_1581753315660.png" alt="文字列表" />
        <div class="tip">对话式FAQ</div>
        <i class="el-icon-circle-check" v-if='componentInfo.type == 1'></i>
      </div>
      <div class='list-type' @click='chooseType(2)'>
        <img src="https://ymm-maliang.oss-cn-hangzhou.aliyuncs.com/ymm-maliang/resource/ymm_1564569032193.png" alt="文字列表" />
        <div class="tip">普通FAQ</div>
        <i class="el-icon-circle-check" v-if='componentInfo.type == 2'></i>
      </div>
    </div>
    <el-card class="box-card" header='基础配置'>
      <div slot="header" class="clearfix">
        <span>列表项</span>
        <el-button style="float: right; padding: 3px 0" type="text" @click='addItem'>新增列表项</el-button>
      </div>
      <div>
        <el-collapse accordion>
          <el-collapse-item :name="i" v-for='(v, i) in componentInfo.list' :key='i'>
            <template slot="title">
              {{i + 1 + '.' + v.name}}<i class="el-icon-delete el-collapse-item__arrow del-item" @click.stop='delItem(i)'></i>
            </template>
            <el-form ref="form" label-width="80px" label-position='right' size='mini'>
              <el-form-item label="设为提问者:" label-width="100px" v-if='componentInfo.type == 1'>
                <el-switch v-model="v.isSender" active-color="#1890ff" inactive-color="#bbbbbb"> </el-switch>
              </el-form-item>
              <el-form-item label="头像:">
                <attr-resource type="image" :url.sync="v.img"></attr-resource>
              </el-form-item>
              <el-form-item label="标题:" v-if='componentInfo.type == 2'>
                <div class="tag">
                  <el-input v-model="v.name" placeholder="请输入标题"></el-input>
                  <el-color-picker v-model="v.titleColor" show-alpha></el-color-picker>
                </div>
              </el-form-item>
              <el-form-item label="标题字号:" v-if='componentInfo.type == 2'>
                    <el-input placeholder="请输入文字大小" v-model="v.titleFont" min='0' type='number'>
                        <template slot="append">px</template>
                      </el-input>
                  </el-form-item>
              <el-form-item label="昵称:" v-if='componentInfo.type == 1'>
                <el-input v-model="v.nick" placeholder="请输入昵称"></el-input>
              </el-form-item>
              <el-form-item label="内容:">
                <attr-richtext :id="i" v-model='v.content'></attr-richtext>
              </el-form-item>
            </el-form>
          </el-collapse-item>
        </el-collapse>
      </div>
    </el-card>
  </div>
</template>

<script>
  export default {
    name: 'maliangeditor',
    props: {
      componentInfo: { // 固定字段，收集所有属性值
        type: [Object],
        default () {
          return {
            type: 1,
            list: []
          }
        }
      }
    },
    data: function () {
      return {
      }
    },
    computed: {
    },
    watch: {
      componentInfo: {
        hanlder: function (v) {
          console.log('editor index', v)
        },
      }
    },
    mounted: function () {
    },
    methods: {
      chooseType (type) {
        this.componentInfo.type = type
        if (type == 1) {
          this.componentInfo.list = [
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
        } else if (type == 2) {
          this.componentInfo.list = [{
            name: '什么是码良？',
            titleColor: '#000',
            titleFont: 16,
            img: 'https://ymm-maliang.oss-cn-hangzhou.aliyuncs.com/ymm-maliang/resource/ymm_1564568927763.png',
            content: '可以快速制作H5页面。无需掌握复杂的编程技术，通过简单拖拽、少量配置即可制作精美的页面，可用于营销场景下的页面制作。'
          }]
        }
      },
      delItem (i) {
        this.componentInfo.list.splice(i, 1)
      },
      addItem () {
        let newArr = this.componentInfo.list.concat(this.componentInfo.list)
        this.componentInfo.list = JSON.parse(JSON.stringify(newArr))
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus" type="text/stylus" scoped>
  .component-editor {
    .list-type{
      margin 10px 0 0 30px
      max-width 250px
      position relative
      border 1px solid #505152
      padding 10px
      border-radius 5px
      img{
        width 100%
      }
      i {
        position absolute
        right 10px
        bottom 15px
        font-size 22px
        color #409EFF
      }
      .tip{
        display: none
        width 100%
        position: absolute
        background-color: rgba(0, 0, 0, 0.7)
        text-align center
        font-size 12px
        padding 3px 0
        bottom 0
        left 0
        color #ccc
      }
      &:hover{
        .tip{
          display: block
        }
      }
    }
    .box-card {
      margin-top 30px
      .tag{
        padding-right 80px
        position relative
        margin-bottom 5px
        .el-color-picker{
          position absolute
          right 30px
          top 0
        }
        i {
          display none
          position absolute
          right 0
          top 5px
          color #F56C6C
          cursor: pointer
        }
        &:hover {
          i {
            display block
          }
        }
      }
    }
  }
</style>
