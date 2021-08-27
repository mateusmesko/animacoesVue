<template>
	<div id="app" class="container-fluid">
		<h1>Animações</h1>
		<hr>
		<b-button variant="primary" class="mb-4" @click="obj1.exibir=!obj1.exibir">teste fade</b-button>
		<transition name="fade"  appear>
			<b-alert variant="info" show v-if="obj1.exibir">{{obj1.msg}}</b-alert>
		</transition>

		<hr>

		<b-button variant="primary" class="mb-4" @click="obj2.exibir=!obj2.exibir">teste slide+fade</b-button>
		<transition name="slide" type="animation" appear>
			<b-alert variant="info" show v-show="obj2.exibir">{{obj2.msg}}</b-alert>
		</transition>
		
		<hr>

		<b-button variant="primary" class="mb-4" @click="obj3.exibir=!obj3.exibir">teste animate</b-button>
		<transition 
		enter-active-class="animated rollIn"
		leave-active-class="animated hinge">
			<b-alert variant="info" show v-show="obj3.exibir">{{obj3.msg}}</b-alert>
		</transition>

		<hr>
		<b-select v-model="tipoAnimacao">
			<option value="fade">Fade</option>
			<option value="slide">Slide</option>
		</b-select>

		<b-button variant="primary" class="mb-4" @click="obj2.exibir=!obj2.exibir">teste slide+fade</b-button>
		<transition :name="tipoAnimacao"  appear mode="out-in">
			<b-alert variant="info" show v-if="obj2.exibir" key="info">{{obj2.msg}}</b-alert>
			<b-alert variant="warning" show v-else key="warn">{{obj2.msg}}</b-alert>
		</transition>
		<hr>
		<button @click="exibir2=!exibir2">mostrar</button>
		<transition
		:css="false"
			@before-enter="beforeEnter"
			@enter="enter"
			@after-enter="afterEnter"
			@enter-cancelled="enterCancelled"
			
			@before-leave="beforeLeave"
			@leave="leave"
			@after-leave="afterLeave"
			@leave-cancelled="leaveCancelled">
			<div v-if="exibir2" class="caixa"></div>
		</transition>
	</div>
</template>

<script>

export default {
	data(){
		return{
			tipoAnimacao:'fade',
			obj1:{
			msg:'uma mensagem de informação para o user',
			exibir:false,
			},
			obj2:{
			msg:'uma mensagem de informação para o user',
			exibir:false,
			},obj3:{
			msg:'uma mensagem de informação para o user',
			exibir:false,
			},
			exibir2:true
		}
	},
	methods: {
		animar(el, done, negativo) {
			let rodada = 1
			const temporizador = setInterval(() => {
				const novaLargura = this.larguraBase + 
					(negativo ? -rodada * 10 : rodada * 10)
				el.style.width = `${novaLargura}px`
				rodada++
				if(rodada > 30) {
					clearInterval(temporizador)
					done()
				}
			}, 20)
		},
		beforeEnter(el) {
			this.larguraBase = 0
			el.style.width = `${this.larguraBase}px`
		},
		enter(el, done) {
			this.animar(el, done, false)
		},
		// afterEnter(el) {
		// 	console.log('afterEnter')
		// },
		// enterCancelled() {
		// 	console.log('enterCancelled')
		// },
		beforeLeave(el) {
			this.larguraBase = 300
			el.style.width = `${this.larguraBase}px`
		},
		leave(el, done) {
			this.animar(el, done, true)
		},
		// afterLeave(el) {
		// 	console.log('afterLeave')
		// },
		// leaveCancelled() {
		// 	console.log('enterCancelled')
		// },
	}
}
</script>

<style>
#app {
	font-family: 'Avenir', Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
	margin-top: 60px;
	font-size: 1.5rem;
}

.caixa{
	height: 300px;
	width: 300px;
	margin: 30px auto;
	background-color: lightgreen;
}

.fade-enter, .fade-leave-to{
	opacity: 0;
}

.fade-enter-active, .fade-leave-active{
	transition: opacity 2s;
}

@keyframes slide-in {
	from{transform: translateY(100%);}
	to{transform: translateY(0)}
}
@keyframes slide-out {
	from{transform: translateY(0);}
	to{transform: translateY(-100%)}
}

.slide-enter, .slide-leave-to{
	opacity: 0;
}

.slide-enter-active{
	animation: slide-in 2s ease;
	transition: opacity 2s;
}

.slide-leave-active{
	animation: slide-out 2s ease;
	transition: opacity 2s;

}
</style>


