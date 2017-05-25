<template>
  <div>
    <Topbar :showArrow="showArrow" :goBack="preView"></Topbar>
    <pulse-loader :loading="loading" :color="color" :size="size" :margin="margin"></pulse-loader>
    <transition name="fade">
      <section v-show="!loading">
        <img v-if="book" :src="book.cover.indexOf('http') === -1 ? staticPath + book.cover : book.cover.slice(book.cover.indexOf('http'))">
        <div class="book-info">
          <p class="book-title" v-if="book">{{book.title}}</p>
          <p class="book-author" v-if="book">{{book.author}}</p>
          <p class="reader-info" v-if="book"><span v-text=""></span>{{wordCount}}万 | {{book.cat}}</p>
        </div>
        <div class="book-operation">
          <button class="btn">追更新</button>
          <button class="btn">开始阅读</button>
        </div>
        <div class="book-status">
          <div class="list-item">
            <span class="item">追书人气</span>
            <span v-if="book">{{book.latelyFollower}}</span>
          </div>
          <div class="list-item">
            <span class="item">读者留存率</span>
            <span v-if="book">{{book.retentionRatio}}%</span>
          </div>
          <div class="list-item">
            <span class="item">日更新字数</span>
            <span v-if="book">{{book.serializeWordCount}}</span>
          </div>
        </div>
        <div class="book-tag" v-if="book">
          <span v-for="(tag, index) in book.tags" :key="index" class="tag">{{tag}}</span>
        </div>
        <p class="book-intro" v-if="book">{{book.longIntro}}</p>
      </section>
      <!--<section></section>-->
    </transition>
  </div>
</template>

<script>
  import Topbar from './Topbar'
  import api from '../libs/api'
  import PulseLoader from 'vue-spinner/src/PulseLoader.vue'
  export default {
    name: 'Book',
    components: {
      Topbar,
      PulseLoader
    },
    data() {
      return {
        showArrow: true,
        book: null,
        staticPath: 'http://statics.zhuishushenqi.com/',
        loading: true,
        color: '#04b1ff',
        size: '10px',
        margin: '4px'
      }
    },
    computed: {
      wordCount() {
        return parseInt(this.book.wordCount / 10000, 10);
      },
      preView(){
          return '/ranklist/'+ this.$store.state.goBackId
      }
    },
    created() {
      //异步获取数据 数据返回前就已经开始渲染组件 会引起渲染组件的报错
      api.getBook(this.$route.params.bookId).then(response => {
        this.book = response.data;
        this.loading = false;
      }, err => {
        console.log(err)
      });
    }
  }

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  img {
    width: 5rem;
    height: 6rem;
    float: left;
    margin-right: 0.4rem;
  }
  
  section {
    box-sizing: border-box;
    padding-right: 1rem;
    padding-left: 1rem;
    padding-bottom: 0.2rem;
    padding-top: 0.2rem;
    margin-top: 3.5rem;
    width: 100vw;
  }
  
  section:first-child {
    margin-bottom: 1rem;
  }
  
  .book-info {
    box-sizing: border-box;
    width: 100%;
    height: 6rem;
    padding-left: 6rem;
    padding-top: 0.5rem;
    padding-bottom: 0.5rem;
  }
  
  .book-info p {
    margin: 0;
    line-height: 1.8rem;
  }
  
  .book-operation {
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    padding-top: 1rem;
    padding-bottom: 1rem;
    border-bottom: 1px solid #f2f2f2;
  }
  
  .book-operation .btn {
    width: 8rem;
    background: #03a9f4;
    border: none;
    color: #fff;
    font-size: 1.1rem;
    text-align: center;
    line-height: 2rem;
    border-radius: .2rem;
  }
  
  .book-operation .btn:focus {
    background: #2196f3;
    outline: none;
  }
  
  .book-status {
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    flex-wrap: wrap;
    padding: 1rem 0;
    border-bottom: 1px solid #f2f2f2;
  }
  
  .list-item {
    display: flex;
    flex-direction: column;
    width: 33%;
    text-align: center;
  }
  
  .item {
    font-size: .8rem;
    color: #807070;
  }
  
  .book-tag {
    display: flex;
    flex-direction: row;
    justify-content: flex-start;
    flex-wrap: wrap;
    padding: .6rem 0 0;
    border-bottom: 1px solid #f2f2f2;
  }
  
  .tag {
    padding: .2rem .7rem;
    color: #fff;
    border-radius: .1rem;
    margin-bottom: .6rem;
    font-size: 0.8rem;
    margin-left: .4rem;
  }
  
  .tag:nth-child(1n) {
    background: burlywood;
  }
  
  .tag:nth-child(2n) {
    background: cadetblue;
  }
  
  .tag:nth-child(3n) {
    background: coral;
  }
  
  .tag:nth-child(4n) {
    background: cornflowerblue;
  }

</style>