<template>
  <div class="hello">
    <h1>TRANSITION 1</h1>
    <button @click="show = !show">Toggle render</button>

    <transition name="slide-fade">
      <p v-show="show">hello</p>
    </transition>

    <h2>TRANSITION 2</h2>

    <button @click="show2 = !show2">Toggle show</button>
    <transition name="bounce">
      <p v-if="show2">
        Lorem ipsum dolor sit amet, consectetur adipiscing elit.
      </p>
    </transition>

    <h2>TRANSITION 3</h2>

    <input v-model="view" type="radio" value="aView" id="a" /><label for="a"
      >A</label
    >
    <input v-model="view" type="radio" value="bView" id="b" /><label for="b"
      >B</label
    >
    <transition name="component-fade" mode="out-in">
      <component :is="view"></component>
    </transition>

    <h2>TRANSITION 4</h2>

    <button @click="flag = !flag">显示/隐藏</button><br />
    <transition
      name="fade"
      @before-enter="beforeEnter"
      @enter="enter"
      @after-enter="afterEnter"
      @enter-cancelled="enterCancelled"
      @before-leave="beforeLeave"
      @leave="leave"
      @after-leave="afterLeave"
      @leave-cancelled="leaveCancelled"
    >
      <div v-show="flag" class="mybtn"></div>
    </transition>

    <h2>TRANSITION 5</h2>
    <span>左右切换：</span>
    <input
      v-model="view2"
      type="radio"
      value="aView"
      id="a"
      @click="view2Toggle"
    /><label for="a2">A</label>
    <input
      v-model="view2"
      type="radio"
      value="bView"
      id="b"
      @click="view2Toggle"
    /><label for="b2">B</label>
    <transition :name="transitionName" mode="out-in">
      <component :is="view2"></component>
    </transition>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import aView from './aView.vue'
import bView from './bView.vue'
export default defineComponent({
  name: 'HelloWorld',
  data() {
    return {
      show: true,
      show2: true,
      view: 'aView',
      view2: 'aView',
      flag: false,
      transitionName: 'transitionRight',
      id: '11',
    }
  },
  components: {
    aView,
    bView,
  },
  methods: {
    view2Toggle() {
      this.transitionName =
        this.transitionName === 'transitionRight'
          ? 'transitionLeft'
          : 'transitionRight'
    },
    // beforeEnter(el: any) {
    //   el.style.opacity = '0'
    // },
    // enter(el: any, done: () => void) {
    //   el.style.opacity = '1'
    // 方法1： 这里不调用，但参数done必须传，不传或调用done的话由于会立即删除元素导致动画不执行
    // 过渡动画会执行，且style 里  display 不为none，不隐藏
    // done();
    // 方法2:
    // setTimeout(done,5000)
    // 方法3:
    //   el.addEventListener('transitionend', function () {
    //     done()
    //   })
    // },

    beforeEnter() {
      alert('beforeEnter 进入过渡状态 开始 前')
    },
    enter(el: any) {
      alert('enter 进入过渡状态 开始')
      setTimeout(() => {
        el.style.backgroundColor = 'green'
      }, 1000)
    },
    afterEnter() {
      alert('afterEnter 进入过渡状态 结束')
    },
    enterCancelled() {
      alert('enterCancelled 进入过渡状态 被打断')
    },

    beforeLeave() {
      alert('beforeLeave 离开过渡状态 开始 前')
    },
    leave(el: any) {
      alert('leave 离开过渡状态 开始')
      el.style.backgroundColor = 'red'
    },
    afterLeave() {
      alert('afterLeave 离开过渡状态 结束')
    },
    leaveCancelled() {
      alert('leaveCancelled 离开过渡状态 被打断')
    },
  },
})
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.slide-fade-enter-active {
  transition: all 0.3s ease-out;
}

.slide-fade-leave-active {
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateX(20px);
  opacity: 0;
}

.bounce-enter-active {
  animation: bounce-in 1s;
}
.bounce-leave-active {
  animation: bounce-in 1s reverse;
}
@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.25);
  }
  100% {
    transform: scale(1);
  }
}

.component-fade-enter-active,
.component-fade-leave-active {
  transition: all 0.5s ease;
}
.component-fade-enter-from,
.component-fade-leave-to {
  opacity: 0;
}

.mybtn {
  width: 100px;
  height: 100px;
  background-color: red;
  transform: translateX(20px);
}
.fade-enter-active,
.fade-leave-active {
  transition: all 2s;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateX(0px);
}
.fade-enter-to,
.fade-leave-from {
  opacity: 1;
  transform: translateX(20px);
}

.transitionLeft-enter-from,
.transitionRight-leave-active {
  /* -webkit-transform: translate(100%, 0); */
  transform: translate(100%, 0);
}
.transitionLeft-leave-active,
.transitionRight-enter-from {
  /* -webkit-transform: translate(-100%, 0); */
  transform: translate(-100%, 0);
}

.transitionLeft-leave-active,
.transitionRight-leave-active,
.transitionLeft-enter-active,
.transitionRight-enter-active {
  transition: all 0.4s ease-out;
}
</style>
