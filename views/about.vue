<template lang="pug">
    transition(v-on:before-enter="beforeEnter" v-on:before-leave="beforeLeave" v-on:enter="enter" v-on:leave="leave" mode="out-in" v-bind:css="false" appear)
        .view(:style="{backgroundColor: color}")
            .inner-right-sidebar
                router-link(to="/").close
                    .icon
                        include ../assets/close.svg
                    span Вернуться на главную
                .links-block
                    ul
                        li.active
                            a(href="#mission") миссия
                        li
                            a(href="#deals") подход
                        li
                            a(href="#directors") руководители
            .container
                iscroll-view.scroll-view(ref="Scrollbar" @scroll="spy" :options="{mouseWheel: true, scrollbars: true, probeType: 3}")
                    section#mission
                        .small-title миссия
                        h2 #[span свежие и натуральные продукты] в каждом доме от мастеров своего дела
                    section#deals
                        .small-title подход
                        h2 «Частная лавочка» — это супермаркет продуктов #[span от частников]
                        p  Сеть Частная Лавочка уделяет отдельное внимание заботе о личном успехе Поставщиков  у аудитории. Мы, в отличие от других, не только  не пытаемся "застроить их под общую гребенку",  но и помогаем в персональном продвижении партнеров-производителей на территории магазинов  и в промоматериалах Частной Лавочки.
                    section#directors
                            .small-title руководители
                            h2 за этот базар #[span отвечают]
                            .people-block
                                .item
                                    img(src="../assets/d1.jpg", alt="")
                                    .content
                                        .name Иванов Константин Борисович
                                        .rank Генеральный директор
                                        .contacts
                                            a(href="tel: 8 903 234–32–34") 8 903 234–32–34
                                            a(href="mailto:ivanov@mail.ru") ivanov@mail.ru
                                        .socials
                                            ul
                                                li 
                                                    a(href="")
                                                        i.fa.fa-facebook(aria-hidden="true")
                                                li 
                                                    a(href="")
                                                        i.fa.fa-vk(aria-hidden="true")
                                                li 
                                                    a(href="")
                                                        i.fa.fa-odnoklassniki(aria-hidden="true")
                                                li 
                                                    a(href="")
                                                        i.fa.fa-instagram(aria-hidden="true")
                                .item
                                    img(src="../assets/d1.jpg", alt="")
                                    .content
                                        .name Иванов Константин Борисович
                                        .rank Генеральный директор
                                        .contacts
                                            a(href="tel: 8 903 234–32–34") 8 903 234–32–34
                                            a(href="mailto:ivanov@mail.ru") ivanov@mail.ru
                                        .socials
                                            ul
                                                li 
                                                    a(href="")
                                                        i.fa.fa-facebook(aria-hidden="true")
                                                li 
                                                    a(href="")
                                                        i.fa.fa-vk(aria-hidden="true")
                                                li 
                                                    a(href="")
                                                        i.fa.fa-odnoklassniki(aria-hidden="true")
                                                li 
                                                    a(href="")
                                                        i.fa.fa-instagram(aria-hidden="true")
                                .item
                                    img(src="../assets/d1.jpg", alt="")
                                    .content
                                        .name Иваноыв Константин Борисович
                                        .rank Генеральный директор
                                        .contacts
                                            a(href="tel: 8 903 234–32–34") 8 903 234–32–34
                                            a(href="mailto:ivanov@mail.ru") ivanov@mail.ru
                                        .socials
                                            ul
                                                li 
                                                    a(href="")
                                                        i.fa.fa-facebook(aria-hidden="true")
                                                li 
                                                    a(href="")
                                                        i.fa.fa-vk(aria-hidden="true")
                                                li 
                                                    a(href="")
                                                        i.fa.fa-odnoklassniki(aria-hidden="true")
                                                li 
                                                    a(href="")
                                                        i.fa.fa-instagram(aria-hidden="true")
                .left-info-block
                    .tag Кто мы
                    h1 супермакет натуральных продуктов
</template>

<script>


    export default {
        data(){
            return{
                color: '#FAF6EB',
                sections: '',
                section_id: []
            }
        },
        methods:{
            beforeEnter(el){
            },
            beforeLeave(el){
            },
            enter(el, done){
                Velocity(document.querySelector('.logo svg'), {width: 125, height: 70}, {duration: 10})
                Velocity(document.querySelectorAll('.logo svg g use'), {fill: '#312217'}, {duration: 350})
                Velocity(document.querySelector('.logo-text'), {color: '#312217'}, {display: 'block'},{duration: 350})
                done();
            },
            leave(el, done){
                done();
            },
            spy(iscroll){
                let top_edge = $('.scroll-view').offset().top;
                let bottom_edge = $('.scroll-view').offset().top + 155;
                $('.scroll-view section').each(function(){
                    let offset = $(this).offset().top;
                    let id = $(this).attr('id');
                    console.log(id);
                    if( offset >= top_edge & offset <= bottom_edge){
                        $('.inner-right-sidebar li').removeClass('active');
                        $('a[href$='+id+']').parent().addClass('active');
                    } 
                    
                })

            },
            // goToSection(event){
                
            // }
        },
        mounted(){
            const iscroll = this.$refs.Scrollbar;
            iscroll.refresh();
            let sections = document.querySelectorAll('.inner-right-sidebar .links-block li a');
            for (let i = 0; i < sections.length; i++) {
                this.section_id.push(sections[i].getAttribute('href'));
            }

            $('.inner-right-sidebar .links-block a').click(function(e){
                e.preventDefault();
                let id = $(this).attr('href');
                let to = $(id).position().top;
                iscroll.scrollBy(0, -to, 250);
                iscroll.refresh();
            });
        
        },
        updated(){

        }
    }
</script>

<style lang="scss">
    //vars
    @import 'src/assets/styles/settings.scss';

    .scroll-down{
        display: none;
    }

    .scroll-view {
        /* -- Attention: This line is extremely important in chrome 55+! -- */
        touch-action: none;
        /* -- Attention-- */
        position: absolute;
        height: 75%;
        width: 70%;
        left: 54%;
        padding: 0px 4%;
        transform: translateX(-50%);
        margin-top: 10%;
        margin-bottom: 5%;
        overflow: hidden;
        h2{
            font-family: bebas;
            font-weight: 800;
            font-size: 5.2vw;
            display: block;
            line-height: 115%;
            margin: 0px;
            margin-bottom: 50px;
            color: $black;
            width: 90%;
            span{
                color: $orange;
            }
        }
        section{
            min-height: 75vh;
        }
        p{
            width: 50%;
            font-size: 18px;
            line-height: 180%;
        }
    }
    .close{
        transform: rotate(90deg);
        width: 255px;
        margin-top: 110px;
        .icon{
            display: inline-block;
            vertical-align: middle;
            margin-right: 30px;
            svg g use{
                fill: #312217;
            }
        }
        span{
            vertical-align: middle;
            display: inline-block;
            font-family: bebas;
            font-weight: 800;
            font-size: 16px;
            color: $gray;
        }
    }
    .inner-right-sidebar{
        position: absolute;
        top: 0px;
        height: 100%;
        right: 10%;
        width: 10%;
        z-index: 2;
        display: flex;
        border-left: 1px solid $side-menu;
        flex-flow: column;
        align-items: center;
        justify-content: space-between;
        padding-top: 42px;
        padding-bottom: 72px;
        .links-block{
            ul{
                list-style: none;
                padding: 0px;
                margin: 0px;
                li{
                   margin-bottom: 28px;
                   font-family: bebas;
                   font-weight: 800;
                   font-size: 16px;
                   &.active{
                       a{
                           color: $brown;
                       }
                   }
                   &:last-child{
                       margin-bottom: 0px;
                   }
                   a{
                       text-decoration: none;
                       transition: all .35s;
                       color: $side-menu;
                       &:hover{
                           color: $side-menu-hover;
                       }
                   }
                }
            }
        }
    }

    .vue-scrollbar__scrollbar-vertical{
        position: absolute;
        top: 0px;
        right: 0px;
        height: 100%;
        .scrollbar{
            background-color: $side-menu;
            width: 5px;
            position: absolute;
            right: 0;
        }
    }
    // .left-info-block h1{
    //     margin-bottom: 0px;
    // }

    .people-block{
        width: 100%;
        display: flex;
        flex-flow: row wrap;
        .item{
            width: 30%;
            margin-right: 3%;
            img{
                width: 100%;
                display: block;
            }
            .content{
                padding: 49px 30px 35px 30px;
                background-color: $light-cyan;
                .name{
                    font-family: bebas;
                    font-weight: 800;
                    font-size: 26px;
                    color: $black;
                    width: 65%;
                    margin-bottom: 34px;
                }
                .rank{
                    font-size: 20px;
                    color: $black;
                    opacity: .4;
                    margin-bottom: 64px;
                }
                .contacts{
                    margin-bottom: 20px;
                    a{
                        font-size: 20px;
                        color: $black;
                        opacity: .4;
                        text-decoration: none;
                        display: inline-block;
                        margin-bottom: 5px;
                        &:hover{
                            text-decoration: underline;
                        }
                    }
                }
                .socials ul{
                    list-style: none;
                    padding: 0px;
                    margin: 0px;
                    li{
                        display: inline-block;
                        margin-right: 15px;
                        &:last-child{
                            margin-right: 0px;
                        }
                        a{
                            width: 30px;
                            height: 30px;
                            border-radius: 50%;
                            background-color: #BAC2C1;
                            display: inline-block;
                            position: relative;
                            i{
                                position: absolute;
                                top: 50%;
                                left: 50%;
                                transform: translate(-50%, -50%);
                                color: $white;
                            }
                        }
                    }
                }
            }
        }
    }

</style>

