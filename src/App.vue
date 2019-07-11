<template>
    <div class="container">
        <div class="row">
            <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
                <h1>Animations</h1>

                <hr>

                <select v-model="alertAnimation" class="form-control">
                  <option value="fade">Fade</option>
                  <option value="slide">Slide</option>
                </select>

                <button class="btn btn-primary mt-10 mb-10" @click="show = !show">Show alert</button>

                <transition :name="alertAnimation">
                  <div class="alert alert-info" v-if="show">
                    This Is some Info
                  </div>
                </transition>

                <transition name="slide" type="animation">
                  <div class="alert alert-info" v-if="show">
                    This Is some Info
                  </div>
                </transition>

                <!-- appear can be used only with simple css, not imported css -->
                <transition
                  enter-active-class="animated bounce"
                  leave-active-class="animated shake">
                  <div class="alert alert-info" v-if="show">
                    This Is some Info
                  </div>
                </transition>

                <transition :name="alertAnimation" mode="out-in">
                  <div class="alert alert-info" v-if="show" key="info">
                    This Is some Info
                  </div>

                  <div class="alert alert-warning" v-else key="warning">
                    This Is some warning
                  </div>
                </transition>

                <hr>

                <button class="btn btn-primary mb-10" @click="load = !load">Load / Remove Element</button>

                <transition
                  @before-enter="beforeEnter"
                  @enter="enter"
                  @after-enter="afterEnter"
                  @enter-canceled="enterCanceled"

                  @before-leave="beforeLeave"
                  @leave="leave"
                  @after-leave="afterLeave"
                  @leave-canceled="leaveCanceled"
                  :css="false">
                  <div style="width:300px; height:100px; background-color: lightgreen" v-if="load"></div>
                </transition>

                <hr>

                <button class="btn btn-primary mb-10" @click="selectedComponent == 'successAlert' ? selectedComponent = 'dangerAlert' :  selectedComponent = 'successAlert'">
                  Togle components
                </button>

                <transition name="fade" mode="out-in">
                  <component :is="selectedComponent"></component>
                </transition>

                <hr>

                <button class="btn btn-primary mt-10 mb-10" @click="addItem">Add item</button>

                <ul class="list-group">
                  <transition-group name="slide">
                    <li
                      class="list-group-item"
                      style="cursor:pointer"
                      v-for="(number, index) in numbers"
                      @click="removeItem(index)"
                      :key="number">
                      {{ number }}
                    </li>
                  </transition-group>
              </ul>

            </div>
        </div>
    </div>
</template>

<script>

    import DangerAlert from './DangerAlert.vue';
    import SuccessAlert from './SuccessAlert.vue';

    export default {
        data() {
            return {
              show: true,
              load: true,
              alertAnimation: 'fade',
              elementWidth: 100,
              selectedComponent: 'successAlert',
              numbers: [1,2,3,4,5,6,7,8],
            }
        },
        methods: {
          beforeEnter(el) {
            this.elementWidth = 100;
            el.style.width = this.elementWidth + 'px';
          },
          enter(el, done) {
            let round = 1
            const interval = setInterval(() => {
              el.style.width = (this.elementWidth + round * 10) + 'px';
              round++;
              if (round > 20) {
                clearInterval(interval);
                done();
              }
            }, 20);
          },
          afterEnter(el) {
          },
          enterCanceled(el) {
          },
          beforeLeave(el) {
            this.elementWidth = 300;
            el.style.width = this.elementWidth + ' px';
          },
          leave(el, done) {
            let round = 1
            const interval = setInterval(() => {
              el.style.width = (this.elementWidth - round * 10) + 'px';
              round++;
              if (round > 20) {
                clearInterval(interval);
                done();
              }
            }, 20);
          },
          afterLeave(el) {
          },
          leaveCanceled(el) {
          },
          addItem() {
            const pos = Math.floor(Math.random() * this.numbers.length);
            this.numbers.splice(pos, 0, this.numbers.length + 1)
          },
          removeItem(index) {
            this.numbers.splice(index, 1);
          }
        },
        components: {
          dangerAlert: DangerAlert,
          successAlert: SuccessAlert
        }
    }
</script>

<style>
  .mt-10 {
    margin-top: 10px;
  }

  .mb-10 {
    margin-bottom: 10px;
  }

  .fade-enter {
    opacity: 0;
  }

  .fade-enter-active {
    transition: opacity 1s;
  }

  .fade-leave {

  }

  .fade-leave-active {
    transition: opacity 1s;
    opacity: 0;
  }

  .slide-enter {
    opacity: 0;

  }

  .slide-enter-active {
    animation: slide-in 1s ease-out forwards;
    transition: opacity .5s;
  }

  .slide-leave {

  }

  .slide-leave-active {
    animation: slide-out 1s ease-out forwards;
    transition: opacity 1s;
    opacity: 0;
    position: absolute;
  }

  .slide-move {
    transition: transform 1s;
  }

  @keyframes slide-in {
    from {
      transform: translateY(20px);
    }

    to {
      transform: translateY(0);
    }
  }

  @keyframes slide-out {
    from {
      transform: translateY(0);
    }

    to {
      transform: translateY(20px);
    }
  }
</style>
