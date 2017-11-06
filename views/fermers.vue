<template lang="pug">
	transition(v-on:before-enter="beforeEnter" v-on:before-leave="beforeLeave" v-on:enter="enter" v-on:leave="leave" mode="out-in" v-bind:css="false" appear)
		.view(:style="{backgroundColor: color}")
			.inner-right-sidebar
				router-link(to="/").close
					.icon
						include ../assets/close.svg
					span Вернуться на главную
			.container(v-if="ferm_data && Object.keys(ferm_data).length")
				iscroll-view.scroll-view(ref="Scrollbar" :options="{mouseWheel: true, scrollbars: true, probeType: 3, disablePointer: true}")
					section
						.people-block
							.item(v-for="fermer in ferm_data.fermers")
								img(:src="fermer.img", alt="")
								.content
									.name {{ fermer.name }}
									p {{ fermer.ferma }}
				.left-info-block
					.tag Частники
					h1 Каталог частников
</template>

<script>
    import axios from 'axios';

	export default {
        data(){
            return{
                color: '#FAF6EB',
                ferm_data: {}
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
    	},
    	created(){
    		const path = this.$route.path;
            const data_src = 'src/data' + path + '/data.json';
            
            axios.get(data_src).
                then(response => {
                    this.ferm_data = response.data;
                }).catch(error => {
                    console.log(error);
                });

            const iscroll = this.$refs.Scrollbar;            
            iscroll.refresh();
    	}
   	}
</script>