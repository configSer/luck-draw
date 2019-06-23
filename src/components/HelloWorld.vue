<template>
  <div class="luck_draw flex flex-hsa flex-hw">
    <div v-for="(item, i) in objArr"
         :class="currentIndex === item.index ? 'single_draw trans' : checkedArr.indexOf(item.index) !== -1 ?  'single_draw trans' :  'single_draw'"
         :key="i"
         :style="singleStyle "
    > 被抽中{{item.checkedNum}}次
    </div>
    <div class="QRCode" :style="QRCodeStyle" @click="goDraw">点击抽奖</div>
  </div>
</template>

<script>
  export default {
    name: 'HelloWorld',
    data() {
      return {
        arr: [1, 2, 3, 4, 5, 6, 7, 8, 16, 24, 32, 40, 48, 47, 46, 45, 44, 43, 42, 41, 33, 25, 17, 9, 10, 11, 12, 13, 14, 15, 23, 31, 39, 38, 37, 36, 35, 34, 26, 18, 19, 22, 30, 27,
          27, 19, 22, 30, 38, 37, 36, 35, 34, 26, 18, 10, 11, 12, 13, 14, 15, 23, 31, 39, 47, 46, 45, 44, 43, 42, 41, 33, 25, 17, 9, 1, 2, 3, 4, 5, 6, 7, 8, 16, 24, 32, 40, 48,
          48, 47, 46, 45, 44, 43, 42, 41, 33, 25, 17, 9, 1, 2, 3, 4, 5, 6, 7, 8, 16, 24, 32, 40, 39, 38, 37, 36, 35, 34, 26, 18, 10, 11, 12, 13, 14, 15, 23, 31, 30, 27, 19, 22,
          22, 30, 27, 19, 11, 12, 13, 14, 15, 23, 31, 39, 38, 37, 36, 35, 34, 26, 18, 10, 2, 3, 4, 5, 6, 7, 8, 16, 24, 32, 40, 48, 47, 46, 45, 44, 43, 42, 41, 33, 25, 17, 9, 1],
        checkedArr: [],
        objArr: [],
        width: document.documentElement.clientWidth,
        height: document.documentElement.clientHeight,
        singleStyle: {width: '', height: ''},
        QRCodeStyle: {left: '', top: '', background: '#ccc', position: 'absolute'},
        currentIndex: 0,
      }
    },
    mounted() {
      this.singleStyle.width = (this.width / 8) - 6 + 'px';
      this.singleStyle.height = (this.height / 6) - 6 + 'px';
      this.QRCodeStyle.width = (this.width / 8) * 2 - 6 + 'px';
      this.QRCodeStyle.height = (this.height / 6) * 2 - 6 + 'px';
      this.QRCodeStyle.left = (this.width / 8) * 3 + 3 + 'px';
      this.QRCodeStyle.top = (this.height / 6) * 2 + 3 + 'px';
      this.setObjArr();
    },
    methods: {
      setObjArr() {
        for (let i = 0; i < 48; i++) {
          this.objArr.push({
            index: i + 1,
            checked: false,
            checkedNum: 0
          });
        }
      },
      random(lower, upper) {
        return Math.ceil(Math.random() * (upper - lower)) + lower;
      },
      goDraw() {
        //可传参,参数位用户信息
        let vm = this;
        let randomNumber = vm.random(44, this.arr.length); //随机下标
        let i = 0;
        let timer = setInterval(function () {
          i++;
          vm.currentIndex = vm.arr[i];
          if (i === randomNumber) {
            clearInterval(timer);
            vm.checkedArr.push(vm.arr[randomNumber]);//已经抽中的数组
            vm.objArr = vm.objArr.map(item => {
              if (item.index === vm.arr[randomNumber]) {
                //将用户信息设置进去便于显示
                item.checked = true;
                item.checkedNum++ //被选择的次数
              }
              return item;
            })
          }
        }, 100);
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
  .luck_draw {
    position: relative;
    width: 100%;
    height: 100%;
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
  }

  .single_draw:not(:nth-of-type(20)):not(:nth-of-type(21)):not(:nth-of-type(28)):not(:nth-of-type(29)) {
    border: 1px solid #ccc;
    /*background:#ccc url("./../../static/timg.jpg") no-repeat;*/
    background-size: cover;
    background-position: center center;
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
  }

  .single_draw {
    margin-top: 3px;
  }

  .trans {
    background-color: rgba(200, 200, 200, .8);
  }
</style>
