<template>
    <div class="container">
        <div class="row">
            <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
                <h1>Animations</h1>
                <hr>
                <select v-model='alertAnimation' class="form-control">
                    <option value="fade">Fade</option>
                    <option value="slide">Slide</option>
                </select>
                <br/>
                <button @click="show =!show" class="btn btn-primary">Show alert</button>
                <br>
                <br>
                <transition :name="alertAnimation" mode="out-in">
                    <div v-if="show" key="info" class="alert alert-info">Some info</div>
                    <div v-else key="warning" class="alert alert-warning">Some warning</div>
                </transition>
                <transition :name="alertAnimation">
                    <div v-show="show" class="alert alert-info">Some info</div>
                </transition>
                <transition name="slide" type="animation" appear>
                    <div v-if="show" class="alert alert-info">Some info</div>
                </transition>
                <transition name="fade" appear>
                    <div v-if="show" class="alert alert-info">Some info</div>
                </transition>

                <transition
                    enter-class=""
                    enter-active-class="animated bounce"
                    leave-class=""
                    leave-active-class="animated shake" >
                    <div v-if="show" class="alert alert-success">Some info</div>
                </transition>
                <hr>
                <h2>JavaScript Animations</h2> 
                <button class="btn btn-primary" @click="load = !load">Load / Remove Element</button>
                <br>
                <br>
                <transition
                    @before-enter="beforeEnter"
                    @enter="enter"
                    @after-enter="afterEnter"
                    @enter-cancelled="enterCancelled"
                    
                    @before-leave="beforeLeave"
                    @leave="leave"
                    @after-leave="afterLeave"
                    @leave-cancelled="leaveCancelled"
                    :css="false">
                    <div v-if="load" style="width: 300px; height: 100px; background-color: lightgreen;"></div>
                </transition>
                <hr>
                <button @click="selectedComponent = selectedComponent === 'app-success-alert' ? 'app-danger-alert' : 'app-success-alert'"  class="btn btn-primary">Toggle Components</button>
                <br><br>
                <transition name="fade" mode="out-in">
                    <component :is="selectedComponent"></component>
                </transition>
                <hr>
                <button class="btn btn-primary" @click="addItem">Add Item</button>
                <br>
                <br>
                    
                <ul class="list-group">
                    <transition-group name="slide">
                        <li
                            class="list-group-item"
                            v-for="(number,index) in numbers"
                            :key="index"
                            style="cursor:pointer;"
                            @click="removeItem(index)">
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
                show: false,
                load: true,
                alertAnimation: 'fade',
                elementWidth: 100,
                selectedComponent: 'app-success-alert',
                numbers: [1,2,3,4,5]
            }
        },
        methods: {
            beforeEnter(el) {
                console.log('beforeEnter')
                this.elementWidth = 100;
                el.style.width = this.elementWidth + 'px';
            },
            enter(el, done) {
                console.log('enter')
                let round = 1;
                const interval = setInterval(() => {
                    el.style.width = (this.elementWidth + round * 10) + 'px'
                    round++
                    if (round > 20) {
                        clearInterval(interval)
                        done()
                    }
                }, 20);
            },
            afterEnter(el) {
                console.log('aferEnter')
            },
            enterCancelled(el) {
                console.log('enterCancelled')
            },
            beforeLeave(el) {
                console.log('beforeLeave')
                
                this.elementWidth = 300
                el.style.width = this.elementWidth + 'px';
            },
            leave(el, done) {
                console.log('leave')
                let round = 1;
                const interval = setInterval(() => {
                    el.style.width = (this.elementWidth - round * 10) + 'px'
                    round++
                    if (round > 20) {
                        clearInterval(interval)
                        done()
                    }
                }, 20);
            },
            afterLeave(el) {
                console.log('afterLeave')

            },
            leaveCancelled(el) {
                console.log('leaveCancelled');
            },
            addItem() {
                const pos = Math.floor(Math.random() * this.numbers.length)
                this.numbers.splice(pos, 0, this.numbers.length + 1)
            },
            removeItem(index) {
                this.numbers.splice(index, 1);
            }
        },
        components: {
            appDangerAlert: DangerAlert,
            appSuccessAlert: SuccessAlert
        }
    }
</script>

<style>
    .fade-enter {
        opacity: 0;
    }

    .fade-enter-active {
        transition: opacity 1s;
    }

    .fade-leave {
        /* opacity: 1; */
    }
    .fade-leave-active {
        transition: opacity 1s;
        opacity: 0;
    }

    .slide-enter {
        opacity: 0;
    }
    .slide-enter-active {
        transition: opacity .5s;
        animation: slide-in 1s ease-out forwards;
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
            tranform: translateY(0);
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
