<template>
	<div class="home-view">
		<div class="home-view__timer" v-for="timer in timers" :key="timer.id">
			<Timer :timer="timer.value" :index="timer.id" @change="changeTime" @remove="removeTimer" />
		</div>
		<AddTimer @click="add" />
	</div>
</template>

<script>
import Timer from '@/components/Timer.vue';
import AddTimer from '@/components/AddTimer.vue';

export default {
	name: 'App',
	components: {
		Timer,
		AddTimer,
	},

	data() {
		return {
			timers: [],
		}
	},

	methods: {
		saveTimers() {
			localStorage.setItem('timers', JSON.stringify(this.timers));
			
		},

		getTimers() {
			this.timers = JSON.parse(localStorage.getItem('timers')) || [{id: 0, value: 0}];
		},

		changeTime(value, index) {
			this.timers[index] = {id: index, value};
			this.saveTimers();
		},
		
		add() {
			this.timers.push({id: this.timers.length, value: 0});
			this.saveTimers();
		},

		removeTimer(index) {
			this.timers = this.timers.filter(x => x.id !== index );
			this.saveTimers();
		}
	},

	mounted() {
		this.getTimers();
	},
}
</script>

<style lang="scss">
	html {
		margin: 0;
		padding: 0;

		body {
			margin: 0;
			padding: 0;

			#app {
				background: #353638;
				min-width: 100vw;
				min-height: 100vh;

				.home-view {
					padding: 50px;
					display: flex;
					flex-flow: row wrap;
					justify-content: center;
					align-items: center;
					gap: 50px;

					&__timer {
						display: flex;
						flex-flow: row wrap;
						gap: 50px;
					}
				}
			}
		}
	}
</style>
