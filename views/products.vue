<template lang="pug">
    transition(v-on:before-enter="beforeEnter" v-on:enter="enter" v-on:leave="leave" v-on:before-leave="beforeLeave" mode="out-in" v-bind:css="false" appear)
        .view(:style="{backgroundColor: color}")
            img.center-plate(src="../assets/woodplate.png", alt="")
            .owl-carousel.product-carousel
                .item(v-for="product in products")
                    router-link(:to="product.link")
                        img(v-bind:src="product.img", v-bind:alt="product.name")
            .container
                .owl-dots
                .owl-arrows
                    a(href="#" @click.prevent="prevSlide")
                        i.fa.fa-chevron-left(aria-hidden="true")
                    a(href="#" @click.prevent="nextSlide")
                        i.fa.fa-chevron-right(aria-hidden="true")
                .left-info-block
                    .tag продукция
                    h1 {{ productText }}
                    router-link(v-bind:to="productLink") подробнее
            .bottom-panel
                a.category-link(v-for="section in sections" @click.prevent="goToSection" href="#" :data-section="section.category" :style="{backgroundColor: section.color}")
                    


</template>

<script>
    export default {
        props:['flow', 'links'],
        data(){
            return{
                color: '#FAF6EB',
                owl: '',
                _flow: '',
                products:[
                    {
                        name: 'проды и растения',
                        img: 'src/assets/m1.png',
                        category: 'veges',
                        link: '/products/veges'
                    },
                    {
                        name: 'Рыба',
                        img: 'src/assets/m2.png',
                        category: 'fishs',
                        link: '/products/fishs'
                    },
                    {
                        name: 'Мясо, шашлыки',
                        img: 'src/assets/m3.png',
                        category: 'meats',
                        link: '/products/meats'
                    },
                    {
                        name: 'Салаты. Готовые блюда',
                        img: 'src/assets/m4.png',
                        category: 'salads',
                        link: '/products/salads'
                    },
                    {
                        name: 'Запасы и заготовки',
                        img: 'src/assets/m5.png',
                        category: 'jams',
                        link: '/products/jams'
                    },
                    {
                        name: 'Хлеб, выпечка, кондитерка',
                        img: 'src/assets/m6.png',
                        category: 'breads',
                        link: '/products/breads'
                    },
                    {
                        name: 'Колбасы, деликатесы',
                        img: 'src/assets/m7.png',
                        category: 'sausages',
                        link: '/products/sausages'
                    },
                    {
                        name: 'Молочная продукция',
                        img: 'src/assets/m8.png',
                        category: 'milks',
                        link: '/products/milks'
                    }

                ],
                sections:[
                    {
                        category: 'veges',
                        color: '#D5DFDE'
                    },
                    {
                        category: 'fishs',
                        color: '#4D3422'
                    },
                    {
                        category: 'meats',
                        color: '#919C6E'
                    },
                    {
                        category: 'salads',
                        color: '#FAF6EB'
                    },
                    {
                        category: 'jams',
                        color: '#DF8061'
                    },
                    {
                        category: 'breads',
                        color: '#AC6A54'
                    },
                    {
                        category: 'sausages',
                        color: '#AB895C'
                    },
                    {
                        category: 'milks',
                        color: '#FFFFFF'
                    }
                ],
                productText: 'плоды и растения',
                productLink: '',
            }
        },
        beforeRouteLeave(to, from, next){
            const toDepth = to.path
            const fromDepth = from.path
            let current;
            let next_view;
            for (let i = 0; i < this.links.length; i++){
                if (toDepth == this.links[i].url){
                    next_view = i;
                }
                if (fromDepth == this.links[i].url){
                    current = i;
                }
            }
            this._flow = current < next_view ? 'forward' : 'back';
            next();
        },
        methods:{
            beforeEnter(el){
                if (this.flow == 'back'){
                    Velocity($(el).find('.product-carousel'), {translateY: '-300%', translateX: '-50%'}, {duration: 10});
                    Velocity($(el).find('.center-plate'), {translateY: '-300%', translateX: '-50%'}, {duration: 10});
                    Velocity($(el).find('.left-info-block'), {translateX: '-150%'}, { duration: 10});
                }
            },
            beforeLeave(el){
                el.style.zIndex = 1;
                Velocity(document.querySelector('.product-carousel'), {translateY: '-50%', translateX: '-50%'}, {duration: 10});
                Velocity(document.querySelector('.center-plate'), {translateY: '-50%', translateX: '-50%'}, {duration: 10});
                Velocity(document.querySelector('.left-info-block'), {translateX: '-0%'}, { duration: 10});
            },
            enter(el, done){
                if (this.flow == 'forward'){
                    Velocity(document.querySelector('.logo svg'), {width: 125, height: 70}, {duration: 10})
                    setTimeout(function(){
                        Velocity(el, {opacity: 1, zIndex: 'initial'}, {duration: 350})
                        Velocity(document.querySelectorAll('.logo svg g use'), {fill: '#312217'}, {duration: 350})
                        Velocity(document.querySelector('.logo-text'), {color: '#312217'}, {display: 'block'},{duration: 350})
                        Velocity(document.querySelector('.bottom-panel'), { bottom: '0px' }, { duration: 350, delay: 350, complete: done})
                    }, 550);
                } else if (this.flow == 'back'){
                    Velocity(document.querySelector('.logo svg'), {width: 125, height: 70}, {duration: 10})
                    setTimeout(function(){
                        Velocity(el, {opacity: 1, zIndex: 'initial'}, {duration: 350})
                        Velocity(document.querySelectorAll('.logo svg g use'), {fill: '#312217'}, {duration: 350})
                        Velocity(document.querySelector('.logo-text'), {color: '#312217'}, {display: 'block'},{duration: 350})
                        Velocity(document.querySelector('.bottom-panel'), { bottom: '0px' }, { duration: 350, delay: 350})
                        Velocity(document.querySelector('.product-carousel'), {translateY: '-50%', translateX: '-50%'}, {duration: 350, delay: 350, complete: done});
                        Velocity(document.querySelector('.center-plate'), {translateY: '-50%', translateX: '-50%'}, {duration: 350, delay: 200});
                        Velocity(document.querySelector('.left-info-block'), {translateX: '-0%'}, { duration: 350});
                    }, 650);
                } else {
                    Velocity(document.querySelector('.logo svg'), {width: 125, height: 70}, {duration: 10})
                    setTimeout(function(){
                        Velocity(el, {opacity: 1, zIndex: 'initial'}, {duration: 350})
                        Velocity(document.querySelectorAll('.logo svg g use'), {fill: '#312217'}, {duration: 350})
                        Velocity(document.querySelector('.logo-text'), {color: '#312217'}, {display: 'block'},{duration: 350})
                        Velocity(document.querySelector('.bottom-panel'), { bottom: '0px' }, { duration: 350, delay: 350, complete: done})
                    }, 350);
                }
            },
            leave(el, done){
                document.querySelector('body').style.backgroundColor = '#D5DFDE';
                if (this._flow == 'forward'){
                    Velocity(document.querySelector('.product-carousel'), {translateY: '-300%', translateX: '-50%'}, {duration: 650});
                    Velocity(document.querySelector('.center-plate'), {translateY: '-300%', translateX: '-50%'}, {duration: 650, delay: 500});
                    Velocity(document.querySelectorAll('.left-info-block'), {translateX: '-150%'}, { duration: 350, delay: 250});
                    setTimeout(()=>{done();}, 1000);
                } else if (this.flow == 'back'){
                    done();
                } else {
                    done();                    
                }
            },
            owlInit(){
                this.owl = $('.product-carousel').owlCarousel({
                    items:6,
                    loop: true,
                    center: true,
                    dotsContainer: '.owl-dots'
                })
            },
            prevSlide(){
			    this.owl.trigger('prev.owl.carousel');
			},
			nextSlide(){
			    this.owl.trigger('next.owl.carousel');
            },
            goToSection(event){
                let links = document.querySelectorAll('.category-link');
                let elem = event.target;
                let index = [].indexOf.call(links, elem);
                console.log(index);
                this.owl.trigger('to.owl.carousel', [index]);
            }
        },
        created(){
    
        },
        mounted(){
            this.owlInit();
            this.owl.on('translate.owl.carousel', (event) => {
                let index = event.page.index;
                var sections = document.querySelectorAll('.category-link');
                this.productText = this.products[index].name;
                this.productLink = this.products[index].link;
                sections.forEach((element) => {
                    element.classList.remove('active');
                   if (element.dataset.section == this.products[index].category){
                        element.classList.add('active');
                   }
                });
            })
        }
    }
</script>


<style lang="scss">
    //vars
    @import 'src/assets/styles/settings.scss';

    .main-logo{
        position: absolute;
        top: 28px;
        left: 48%;
        transform: translateX(-50%);
        z-index: 2;
        text-align: center;
        svg g use {
            fill: $brown;
        }
    }
    .logo-text{
        font-family: bebas;
        font-weight: 800;
        font-size: 13px;
        letter-spacing: 1.5px;
        color: $brown;
        text-align: center;
        display: block;
        opacity: 1;
    }
     .center-plate{
        position: absolute;
        top: 55%;
        left: 50%;
        transform: translateY(-50%) translateX(-50%);
        z-index: 1;
    }
    .left-info-block{
        position: absolute;
        bottom: 8%;
        left: 3%;
        width: 250px;
        .tag{
            font-family: bebas;
            font-weight: 800;
            font-size: 20px;
            color: $gray;
            margin-bottom: 50px;
        }
        h1{
            font-family: bebas;
            font-weight: 800;
            font-size: 40px;
            line-height: 120%;
            margin: 0px;
            margin-bottom: 50px;
            color: $brown;
        }
        a{
            font-family: bebas;
            font-weight: 800;
            font-size: 20px;
            color: $black;
            &:hover{
                text-decoration: none;
            }
        }
    }

    .product-carousel{
        position: absolute;
        height: 35%;
        width: 104%;
        top: 50%;
        left: 48%;
        transform: translateY(-50%) translateX(-50%);
        cursor: move;
        .item{
            text-align: center;
            display: block;
            position: relative;
            height: 100%;
            img{
                width: 130px;
                display: inline-block;
                position: absolute;
                top: 50%;
                left: 50%;
                transform: translate(-50%,-50%);
            }
        }
    }

    .owl-dots{
        position: absolute;
        top: 3%;
        right: 3%;
        .owl-dot{
            width: 10px;
            height: 10px;
            padding: 5px;
            border-radius: 50%;
            display: inline-block;
            background-color: $black-brown;
            opacity: 0.3;
            margin-right: 20px;
            cursor: pointer;
            transition: all .35s;
            &.active, &:hover{
                opacity: 1;
            }
            &:last-child{
                margin-right: 0px;
            }
        }
    }
    .owl-arrows{
        position: absolute;
        bottom: 8%;
        right: 3%;
        a{
            color: $black-brown;
            margin-right: 30px;
            font-size: 20px;
            &:last-child{
                margin-right: 0px;
            }
        }
    }
    .bottom-panel{
        position: absolute;
        /* bottom: 0px; */
        left: 0px;
        width: 100%;
        height: 40px;
        display: flex;
        z-index: 2;
        flex-flow: row nowrap;
        a{
            width: 12.5%;
            height: 100%;
            transition: all .35s;
            display: inline-block;
            position: relative;
            &:before{
                content: '';
                position: absolute;
                bottom: 100%;
                height: 0px;
                width: 100%;
                background-color: inherit;
                transition: all .35s;
            }
            &.active{
                &:before{
                    height: 20px;
                }
            }
        }
    }

</style>


