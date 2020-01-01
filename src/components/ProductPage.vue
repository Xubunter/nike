<template>
  <div class="page">
    <div class="header">
      <div class="menu-btn" @click="slidebar=!slidebar">
        <div class="line"></div>
        <div class="line"></div>
        <div class="line" :class="{ 'line-a': slidebar }"></div>
      </div>
      <div class="logo"><img src="https://img.icons8.com/ios-filled/100/000000/nike.png" height="50"></div>
      <div class="search"><img src="https://img.icons8.com/ios-glyphs/30/000000/search.png"></div>
    </div>
    <div class="menu">
      <transition name="sidebar">
        <div class="sidebar" v-show="slidebar">
          <div class="nav-item">Shoes</div>
          <div class="nav-item">Products</div>
          <div class="nav-item">Games</div>
          <div class="nav-item">Player</div>
          <div class="nav-item">Sponsers</div>
        </div>
      </transition>
    </div>
    <div class="wrapper"
         v-touch:swipe.left="tt"
         v-touch:swipe.right="tt"
    >

      <transition :name="slide" mode="out-in">
        <div class="section"
             v-for="i in 6"
             :key="i-1"
             v-if="i-1===page"

        >
          <div class="product-description">
            <h1 class="title">Nike Epic</h1>
            <h2 class="subtitle">React Flyknit</h2>
            <p class="description">The Nike Epic React Flyknit Men's Running Shoe provides crazy
              comfort that lasts as long as you can run. Its Nike React foam cushioning is responsive
              yet lightweight, durable yet soft. This attraction of opposites creates a sensation that
              not only enhances the feeling of moving forward, but makes running feel fun, too.
            </p>
            <div class="settings">
              <div class="shoe-size">
                <ShoeSize></ShoeSize>
              </div>
              <div class="line" style="width: 2px; height: 40px; background: red; border-radius: 1px; margin: 0 20px"></div>
              <div class="colors">
                <div class="color-item" style="background: #339de5" :class="{active: color==='blue'}" @click="color='blue'"></div>
                <div class="color-item" style="background: #b04cda" :class="{active: color==='orange'}" @click="color='orange'"></div>
              </div>
            </div>
            <div class="btn-add" @click="addToCard">
              <div class="label"><img src="../assets/cart.svg" alt="">Add to card</div>
                <div class="counter">
                  <transition name="cart-counter" mode="out-in">
                    <span key="1" v-if="counterA">{{card}}</span>
                    <span key="2" v-else>{{card}}</span>
                  </transition>
                </div>
            </div>
          </div>
          <div class="photo-container">
            <div class="product-photo">
              <transition name="big-ellipse">
                <div class="ellipse" :class="color" id="big" v-if="color==='blue'" key="1"></div>
                <div class="ellipse" :class="color" id="big" v-else key="2"></div>
              </transition>
              <transition name="small-ellipse">
                <div class="ellipse" :class="color" id="small" v-if="color==='blue'" key="1"></div>
                <div class="ellipse" :class="color" id="small" v-else key="2"></div>
              </transition>
              <div class="img">
                <transition name="img" mode="out-in">
                  <img src="../assets/Layer_1.png" alt="" v-if="color==='blue'" key="1">
                  <img src="../assets/Layer_2.png" alt="" v- key="2">
                </transition>
              </div>
            </div>
          </div>
        </div>
      </transition>
    </div>

    <div class="footer">
      <div class="discover">
        designed by <a href="https://www.uplabs.com/mahdifakhr" target="_blank" style="text-decoration: none; color:#000000">@MahdiFakhr</a>
      </div>
      <div class="pagination">
        <div class="btn-pag" :class="{'btn-pag-active': page>0}" @click="move(-1)">
          <div class="left arrow-pag" :class="{'arrow-pag-active': page>0}"></div>
        </div>
        <div class="btn-pag" :class="{'btn-pag-active':  page<maxPage}" @click="move(1)">
          <div class="right arrow-pag" :class="{'arrow-pag-active': page<maxPage}"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ShoeSize from './ShoeSize.vue';

export default {
  name: 'ProductPage',
  components: {
    ShoeSize,
  },
  data() {
    return {
      slidebar: false,
      color: 'blue',
      card: 0,
      counterA: true,
      page: 0,
      maxPage: 5,
      toRight: true,
    };
  },
  computed: {
    slide() {
      return this.toRight ? 'slide-toright' : 'slide-toleft';
    },
  },
  methods: {
    test() {
      if (this.color === 'blue') {
        this.color = 'orange';
      } else {
        this.color = 'blue';
      }
    },
    addToCard() {
      this.card += 1;
      this.counterA = !this.counterA;
    },
    move(n) {
      const newPage = this.page + n;
      if (newPage >= 0 && newPage <= this.maxPage) {
        this.page = newPage;
        this.toRight = n > 0;
      }
    },
    tt(m) {
      if (m === 'right') {
        this.move(-1);
      }
      if (m === 'left') {
        this.move(1);
      }
    },
  },
};
</script>

<style lang="scss">
  .page{
    height: 100vh;
    width: 100%;
    box-sizing: border-box;
    padding: 7.4vh;
    display: flex;
    flex-direction: column;
    overflow: hidden;
    user-select: none;
  }
  .header{
    display: flex;
    justify-content: space-between;
    align-items: center;
    z-index: 100000;
    .search, .logo{
      cursor: pointer;
    }
    .menu-btn{
      cursor: pointer;
      .line{
        height: 3px;
        width: 30px;
        border-radius: 10px;
        margin: 5px 0;
        background-color: #000;
        transition: width .2s ease;
        &-a{
          width: 20px;
        }
      }
    }
  }
  .wrapper{
    display: flex;
  }
  .menu{
    .sidebar{
      height: 100vh;
      box-sizing: border-box;
      display: flex;
      flex-direction: column;
      justify-content: center;
      top:0;
      position: fixed;
      z-index: 1000;
      .nav-item{
        text-transform: uppercase;
        cursor: pointer;
        margin: 20px 0;
      }
    }
  }
  .section{
    padding: 7rem 0 0 9rem;
    box-sizing: border-box;
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 100%;
    .product-description{
      .title{
        font-size: 5rem;
        line-height: 4rem;
        font-weight: 800;
        letter-spacing: 1.58px;
        text-transform: uppercase;
        color: #ff1d1d;
        margin: 0;
      }
      .subtitle{
        font-size: 5rem;
        font-weight: 600;
        letter-spacing: 1.58px;
        line-height: 4rem;
        text-transform: uppercase;
        color: #151515;
        margin: 0;
      }
      .description{
        font-size: 0.8rem;
        font-weight: 400;
        line-height: 1.25rem;
        letter-spacing: -0.25px;
        color: #131e20;
        margin-left: 6px;
        max-width: 43.75rem;
      }
      .settings{
        display: flex;
        justify-content: space-between;
        align-items: center;
        .colors{
          display: flex;
          .color-item{
            width: 38px;
            height: 38px;
            margin-right: 27px;
            border-radius: 100%;
            cursor: pointer;
            transition: 0.2s;
            &:hover{
              transform: scale(1.2);
            }
          }
        }
        .active{
          transform: scale(1.2);
        }
      }
      .btn-add{
        height: 48px;
        width: 240px;
        border-radius: 24px;
        background-color: #ff0000;
        padding: 0 11px 0 33px;
        margin-top: 60px;
        box-sizing: border-box;
        display: flex;
        justify-content: space-between;
        align-items: center;
        user-select: none;
        color: white;
        cursor: pointer;
        transition: .2s;
        &:hover{
          background-color: rgba(91, 182, 223, 1);
        }
        &:active{
          transition: none;
          background-color: rgba(91, 182, 223, 0.7);
        }
        .counter{
          border-radius: 100%;
          background-color: rgba(50, 50, 50, 0.1);
          display: flex;
          justify-content: center;
          align-items: center;
          height: 36px;
          width: 36px;
          overflow: hidden;
        }
        .label{
          display: flex;
          align-items: center;
          img{
            margin-right: 10px;
          }
        }
      }

    }
    .product-photo{
      position: relative;
      display: inline-flex;
      justify-self: flex-end;
      padding-left: 2rem;
      box-sizing: border-box;
      min-height: 550px;
      .ellipse{
        border-radius: 100%;
        position: absolute;
      }
      .blue#big{
        width: 550px;
        height: 550px;
        background-color: #5bb6df;
        z-index: 0;
        top: -100px;
        left: 140px;
      }
      .blue#small{
        width: 310px;
        height: 310px;
        background-color: #ff1d1d;
        z-index: 0;
        top: 90px;
      }
      .orange#big{
        width: 550px;
        height: 550px;
        background-color: #d80376;
        z-index: 0;
        top: -100px;
        left: 140px;
      }
      .orange#small{
        width: 310px;
        height: 310px;
        background-color: #ffd40d;
        z-index: 0;
        top: 90px;
      }
      .img{
        position: relative;
        width: 766px;
        z-index: 100;
      }
    }

  }
  .footer{
    display: flex;
    margin-top: auto;
    justify-content: space-between;
    align-items: flex-end;
    margin-right: -7.4vh;
    .pagination{
      display: flex;
      .btn-pag{
        height: 56px;
        width: 56px;
        background-color: #f7f7f7;
        display: flex;
        justify-content: center;
        align-items: center;
        cursor: pointer;
        .arrow-pag{
          height: 12px;
          width: 12px;
          border-left: 2px solid #151515;
          border-top: 2px solid #151515;
        }
        .left{
          transform: rotate(-45deg);
        }
        .right{
          transform: rotate(135deg);
        }
        .arrow-pag-active{
          border-color:#ffffff;
        }
      }
      .btn-pag-active{
        background-color: #ff1d1d;
        transition: 0.3s;
        &:active{
          background-color:#5bb6df;
        }
      }
    }
  }
  .big-ellipse-enter-active, .big-ellipse-leave-active {
    transition: all .5s;
  }
  .big-ellipse-enter{
    transform: scale(0.2);
  }
  .big-ellipse-leave-to /* .fade-leave-active до версии 2.1.8 */ {
    transform: scale(0.2);
    opacity: 0;
  }

  .small-ellipse-enter-active, .small-ellipse-leave-active {
    transition: all .5s;
  }
  .small-ellipse-enter{
    transform: scale(0.2);
  }
  .small-ellipse-leave-to /* .fade-leave-active до версии 2.1.8 */ {
    transform: scale(0.2);
    opacity: 0;
  }

  .sidebar-enter-active, .sidebar-leave-active {
    transition: all .5s;
  }
  .sidebar-enter, .sidebar-leave-to /* .fade-leave-active до версии 2.1.8 */ {
    transform: translateX(-100px);
    opacity: 0;
  }
  .img-enter-active{
    transition: all .5s;
  }
  .img-leave-active {
    transition: all .2s;
  }
  .img-enter{
    transform: rotate(40deg) scale(0.2);
    opacity: 0;
  }
  .img-leave-to /* .fade-leave-active до версии 2.1.8 */ {
    transform: scale(0.5);
    opacity: 0;
  }

  .cart-counter-enter-active{
    transition: all .2s ease;
  }
  .cart-counter-leave-active {
    transition: all .2s ease;
  }
  .cart-counter-enter{
    transform: translateY(-30px) ;
    opacity: 0.5;
  }
  .cart-counter-leave-to /* .fade-leave-active до версии 2.1.8 */ {
    transform: translateY(30px) ;
    opacity: 0.5;
  }

  .slide-toright-enter-active {
    transition: all .2s ease;
  }
  .slide-toright-leave-active {
    transition: all .2s cubic-bezier(1.0, 0.5, 0.8, 1.0);
  }
  .slide-toright-enter {
    transform: translateX(50vw);
    opacity: 0;
  }
  .slide-toright-leave-to {
    transform: translateX(-50vw);
    opacity: 0;
  }


  .slide-toleft-enter-active {
    transition: all .2s ease;
  }
  .slide-toleft-leave-active {
    transition: all .2s cubic-bezier(1.0, 0.5, 0.8, 1.0);
  }
  .slide-toleft-enter {
    transform: translateX(-50vw);
    opacity: 0;
  }
  .slide-toleft-leave-to {
    transform: translateX(50vw);
    opacity: 0;
  }

  @media (max-width: 1550px) {
    .product-description{
      .settings{
        flex-direction: column-reverse;
        .line{
          display: none;
        }
      }
    }
    .photo-container{
      position: relative;
      display: inline-flex;
      #big{
        height: 440px !important;
        width: 440px !important;
      }
      #small{
        height: 210px !important;
        width: 210px !important;
      }
      .product-photo{
      }
      img, .img{
        width: 600px !important;
      }
    }
  }
  @media (max-width: 1550px) {
    .page{
      padding: 2rem;
    }
    .footer{
      margin-right: -2rem;
    }
    .section{
      padding-left: 0;
    }
    .sidebar{
      padding-right: 2rem;
      background-color: #fff;
    }
  }
  @media (max-width: 1050px) {
    .page{
      height: 100% !important;
    }
    .wrapper{
      margin-top: auto;
    }
    .footer{
      .btn-pag{
        height: 80px !important;
        width: 80px !important;
      }
      .ar-pag{
        height: 22px !important;
        width: 22px !important;
      }
    }
    .menu{
      overflow: hidden !important;
    }
    .sidebar{
      width: 100%;
      left: 0;
      padding: 0 !important;
      align-items: center;
      position: absolute !important;
      .nav-item{
        font-size: 2.5rem;
        margin: 40px 0 !important;
      }
    }
    .section{
      flex-direction: column-reverse;
    }
    .product-description{
      .settings{
        flex-direction: row;
        .line{
          display: block;
        }
      }
      .description{
        font-size: 1.4rem !important;
        line-height: 1.8rem !important;
      }
    }
    .product-photo{
      min-height: 420px;
      margin-bottom: 10px;
    }
  }
  @media (max-width: 685px){
    html{
      font-size: 14px;
    }
    .photo-container{
      #big{
        height: 340px !important;
        width: 340px !important;
        top: -60px !important;
        left: 140px !important;
      }
      #small{
        height: 210px !important;
        width: 210px !important;
        top: 70px !important;
      }
      .product-photo{
        min-height: 320px;
      }
      img, .img{
        width: 460px !important;
      }
    }
  }
  @media (max-width: 585px){
    .photo-container{
      #big{
        height: 240px !important;
        width: 240px !important;
        top: -60px !important;
        left: 140px !important;
      }
      #small{
        height: 150px !important;
        width: 150px !important;
        top: 70px !important;
      }
      .product-photo{
        min-height: 250px;
      }
      img, .img{
        width: 360px !important;
      }
    }
    .settings{
      flex-direction: column-reverse !important;
      .line{
        display: none !important;
      }
    }
    .btn-add{
      margin: 10px auto !important;
    }
    .product-description{
      margin-bottom: 50px;
    }
  }

</style>
