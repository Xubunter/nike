<template>
  <div class="number-section">
    <div class="buttons">
      <div class="arrow" @click="move(-1)">&#8592;</div>
      <div class="arrow"  @click="move(1)">&#8594;</div>
    </div>
    <div class="sizes" >
      <div class="box" :style="{ transform: `translateX(${shift}px)` }">
                <span class="size"
                      v-for="(size, i) in sizes"
                      :key="size"
                      :class="{active: i === active, 'sub-active': i === active-1 || i === active+1, 'non-active': i <active-1 || i>active+1}"
                      @click="active=i"
                >
          {{size}}
        </span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ShoeSize',
  data() {
    return {
      sizes: [37, 38, 39, 40, 42, 43, 44, 45, 46],
      active: 1,
    };
  },
  computed: {
    shift() {
      return (3 - this.active) * 40 + 20;
    },
  },
  methods: {
    move(n) {
      const newActive = this.active + n;
      if (newActive >= 0 && newActive < this.sizes.length){
        this.active = newActive;
      }
    },
    test() {
      console.log(window.scrollY > 0);
    },
  },
};
</script>

<style lang="scss" scoped>
  .number-section{
    width: 340px;
    user-select: none;
  }
  .buttons{
    display: flex;
    justify-content: space-between;
    .arrow{
      font-size: 24px;
      cursor: pointer;
    }
  }
  .box{
    width: 1000px;
    height: 44px;
    display: flex;
    align-items: center;
    transition: 0.5s ease;
  }
  .sizes{
    width: 340px;
    height: 60px;
    overflow: hidden;
    .size{
      margin: 0 10px;
      transition: 0.3s ease;
      line-height: 50px;
      cursor: pointer;
    }
    .non-active{
      font-size: 16px;
      opacity: 0.16;
    }
    .active{
      font-size: 24px;
    }
    .sub-active{
      font-size: 16px;
      opacity: 0.5;
    }
  }
  @media (max-width: 1050px){
    .arrow{
      font-size: 36px !important;
    }
  }
</style>
