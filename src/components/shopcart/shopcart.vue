<template>
	<div class="shopcart">
		<div class="content"  @click="toggleList">
			<div class="content-left">
				<div class="logo-wrapper">
					<div class="logo" :class="{'logoHave':totalCount>0}">
						<span class="icon-shopping_cart"></span>
					</div>
					<div class="num" v-show="totalCount>0">{{totalCount}}</div>
				</div>
				<div class="price">￥{{totalPrice}}</div>
				<div class="desc">另需配送费￥{{deliveryPrice}}元</div>
			</div>
			<div class="content-right">
				<div class="pay" :class="payClass">
					{{payDesc}}
				</div>
			</div>
		</div>
    <div class="ball-conyainer">
      <!--<transition name="ball">-->
        <!--<div v-for="ball in balls" v-show="ball.show" class="ball">-->
          <!--<div class="inner inner-hook"></div>-->
        <!--</div>-->
      <!--</transition>-->
      <transition name="fold">
        <div class="shopcart-list" v-show="showCart&&totalCount">
          <div class="list-header">
            <h1 class="title">购物车</h1>
            <span class="empty">清空</span>
          </div>
          <div class="list-content">
            <ul>
              <li class="food" v-for="food in selectFoods">
                <span class="name">{{food.name}}</span>
                <div class="price">
                  <span >￥{{food.price*food.count}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                  <cartcontrol :food="food"></cartcontrol>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </transition>
    </div>
	</div>
</template>

<script>
  import cartcontrol from '../../components/cartcontrol/cartcontrol';

	export default {
		props: {
			selectFoods: {
				type: Array,
				default() {
					return [
						{
						}
					];
				}
			},
			data() {
			  return {
//			      balls: [
//              {
//			          show: false
//              },
//              {
//                show: false
//              },
//              {
//                show: false
//              },
//              {
//                show: false
//              },
//              {
//                show: false
//              }
//            ],
//            dropBall: []
        };
      },
			deliveryPrice: {
				type: Number,
				default: 0
			},
			minPrice: {
				type: Number,
				default: 0
			}
		},
		data() {
		    return {
		        showCart: false
      };
    },
		computed: {
			totalPrice() {
				let total = 0;
				this.selectFoods.forEach((food) => {
					total += food.price * food.count;
				});
				return total;
			},
			totalCount() {
				let count = 0;
				this.selectFoods.forEach((food) => {
					count += food.count;
				});
				return count;
			},
			payDesc() {
				if (this.totalPrice === 0) {
					return `￥${this.minPrice}元起送`;
				} else if (this.totalPrice < this.minPrice) {
					let diff = this.minPrice - this.totalPrice;
					return `还差￥${diff}元起送`;
				} else {
					return '去结算';
				}
			},
			payClass() {
				if (this.totalPrice < this.minPrice) {
					return 'not-enough';
				} else {
					return 'enough';
				}
			}
//      listShow() {
//			    console.log(this.totalCount);
//			    if (!this.totalCount) {
//			        /* 购物车内没东西 */
//			        this.fold = true;
//			        return false;
//          }
//          /* 购物车内有东西 */
//          let show = !this.fold;
//			    return show;
//      }
		},
    methods: {
//		    drop(el) {
//		        for (let i = 0; i < this.balls.length; i++) {
//		            let ball = this.balls[i];
//		            if (!ball.show) {
//		                ball.show = true;
//		                ball.el = el;
//		                this.dropBall.push(ball);
//		                return;
//                }
//            }
//        },
        toggleList() {
          if (!this.totalCount) {
              /* 购物车内没东西 */
              return;
          }
          /* 购物车内有东西 */
          this.showCart = !this.showCart;
        }
    },
//    transitions: {
//		    drop: {
//		        beforeEnter(el) {
//              let count = this.balls.length;
//              while (count--) {
//                  let ball = this.balls[count];
//                  if (ball.show) {
//                      let rect = ball.el.getBoundingClientRect();
//                      let x = rect.left - 32;
//                      let y = -(window.innerHeight - rect.top - 22);
//                      el.style.display = '';
//                      el.style.webkitTransform = `translate3d(0,${y}px,0)`;
//                      el.style.transform = `translate3d(0,${y}px,0)`;
//                      let inner = el.getElementsByClassName('inner-hook')[0];
//                      inner.style.webkitTransform = `translate3d(${x}px,0,0)`;
//                      inner.style.transfform = `translate3d(${x}px,0,0)`;
//                  }
//              }
//            },
//            enter(el) {
//              /* eslint-disable no-unused-vars */
//              let rf = el.offsetHeight;
//              this.$nextTick(() => {
//                el.style.display = '';
//                el.style.webkitTransform = `translate3d(0,0,0)`;
//                el.style.transform = `translate3d(0,0,0)`;
//                let inner = el.getElementsByClassName('inner-hook')[0];
//                inner.style.webkitTransform = `translate3d(0,0,0)`;
//                inner.style.transfform = `translate3d(0,0,0)`;
//              });
//            },
//            afterEnter(el) {
//              let ball = this.dropBalls.shift();
//              if (ball) {
//                  ball.show = false;
//                  el.styl.display = 'none';
//              }
//            }
//        }
//    }
    components: {
		    cartcontrol
    }
	};
</script>

<style>
	.shopcart{
		position: fixed;
		left: 0;
		bottom: 0;
		z-index: 50;
		width: 100%;
		height: 48px;
	}
	.shopcart .content{
		display: flex;
		background: #141d27;
		font-size: 0;
	}
	.shopcart .content .content-left{
		flex: 1;
	}
	.shopcart .content .content-left .logo-wrapper{
		display: inline-block;
		position: relative;
		top: -10px;
		margin: 0 12px;
		padding: 6px;
		width: 56px;
		height: 56px;
		box-sizing: border-box;
		vertical-align: top;
		border-radius: 50%;
		background: #141d27;
	}
	.shopcart .content .content-left .logo-wrapper .logo{
		width: 100%;
		height: 100%;
		border-radius: 50%;
		text-align: center;
		background: #2b343c;
		color: #80858a;
	}
	.shopcart .content .content-left .logo-wrapper .logo .icon-shopping_cart{
		font-size: 24px;
		line-height: 44px;
	}
	.shopcart .content .content-left .logo-wrapper .logoHave{
		background: dodgerblue;
		color: white;
	}
	.shopcart .content .content-left .num{
		position: absolute;
		top: 0;
		right: 0;
		width: 24px;
		height: 16px;
		line-height: 16px;
		text-align: center;
		border-radius: 16px;
		font-size: 9px;
		font-weight: 700;
		color: #FFF;
		background: rgb(240,20,20);
		box-shadow: 0 4px 8px 0 rgba(0,0,0,0.4);
	}
	.shopcart .content .content-left .price{
		display: inline-block;
		vertical-align: top;
		margin-top: 12px;
		line-height: 24px;
		box-sizing: border-box;
		padding-right: 12px;
		border-right: 1px solid rgba(255,255,255,0.1);
		font-size: 16px;
		font-weight: 700;
		color: rgba(255,255,255,0.4);
	}
	.shopcart .content .content-left .desc{
		display: inline-block;
		vertical-align: top;
		line-height: 24px;
		margin: 12px 0 0 12px;
		font-size: 10px;
		color: rgba(255,255,255,0.4);
	}
	.shopcart .content .content-right{
		flex: 0 0 105px;
		width: 105px;
	}
	.shopcart .content .content-right .pay{
		height: 48px;
		line-height: 48px;
		text-align: center;
		font-size: 12px;
		color: rgba(255,255,255,0.4);
		font-weight: 700;
	}
	.shopcart .content .content-right .not-enough{
		background: #2b333b;
	}
	.shopcart .content .content-right .enough{
		background: #00b43c;
		color: white;
	}
  .shopcart .ball-conyainer{

  }
  .shopcart .ball-conyainer .ball{
    position: fixed;
    left: 32px;
    bottom: 22px;
    z-index: 200;
  }
  /*.ball-enter-active, .ball-leave-active {*/
    /*transition: all 0.4s cubic-bezier(0.49,-0.29,0.75,0.41);*/
  /*}*/
  /*.ball-enter, .ball-leave-active {*/

  /*}*/
  .shopcart .ball-conyainer .ball .inner{
    width: 16px;
    height: 16px;
    border-radius: 50%;
    background: rgb(0, 160, 220);
    transition: all 0.4s linear;
  }
  .shopcart-list{
    position: absolute;
    top: 0;
    left: 0px;
    z-index: -1;
    width: 100%;
  }
  .fold-enter-active, .fold-leave-active {
    transition: all 0.5s;
    transform: translateY(-100%);
  }
  .fold-enter, .fold-leave-active {
    transform: translateY(0);
  }
  .shopcart-list .list-header{
    height: 40px;
    line-height: 40px;
    padding: 0 18px;
    background: #f3f5f7;
    border-bottom: 1px solid rgba(7,17,27,0.1);
  }
  .shopcart-list .list-header .title{
    float: left;
    font-size: 14px;
    color: rgb(7,17,27);
  }
  .shopcart-list .list-header .empty{
    float: right;
    font-size: 12px;
    color: rgb(0,160,220);
  }
  .shopcart-list .list-content{
    padding: 0 18px;
    max-height: 217px;
    background: #fff;
    overflow: hidden;
  }
  .shopcart-list .list-content .food{
    position: relative;
    padding: 12px 0;
    box-sizing: border-box;
    border-bottom: 1px solid rgba(7,17,27,0.1);
  }
  .shopcart-list .list-content .food .name{
    line-height: 24px;
    font-size: 14px;
    color: rgb(7,17,27);
  }
  .shopcart-list .list-content .food .price{
    position: absolute;
    right: 90px;
    bottom: 12px;
    line-height: 24px;
    font-width: 700;
    font-size: 14px;
    color: rgb(240,20,20);
  }
  .shopcart-list .list-content .food .cartcontrol-wrapper{
    position: absolute;
    right: 0px;
    bottom: 6px;
  }
</style>
