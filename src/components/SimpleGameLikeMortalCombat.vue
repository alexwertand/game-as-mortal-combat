<template>
	<section class="main-section">
		<h1 class="main-heading">{{ mainHeadingText }}</h1>

		<section class="row battle-section">
			<div class="column player-section">
				<h2 class="player-name text-center">{{ nameOfFirstPlayer }}</h2>

				<div class="healthbar">
					<div 
						class="healthbar text-center"
						style="display: block; background-color: green;"
						:style="{width: healthOfFirstPlayer + '%'}">
						{{ healthOfFirstPlayer }}
					</div>
				</div>
			</div>

			<div class="column player-section">
				<h2 class="player-name text-center">{{ nameOfSecondPlayer }}</h2>

				<div class="healthbar">
					<div 
						class="healthbar text-center"
						style="display: block; background-color: green;"
						:style="{width: healthOfSecondPlayer + '%'}">
						{{ healthOfSecondPlayer }}
					</div>
				</div>
			</div>
		</section>

		<section class="row controls" v-if="!gameIsRunning">
			<div class="contrlos-wrapper text-center">
				<button 
						id="start-game"
						class="btn"
						@click="startGame">{{ startGameText }}</button>
			</div>
		</section>

		<section class="row controls" v-else>
			<div class="contrlos-wrapper text-center">
				<button 
						id="attack"
						class="btn"
						@click="attack">{{ attackText }}</button>

				<button
						id="special-attack"
						class="btn"
						@click="specialAttack">{{ specialAttackText }}</button>

				<button
						id="heal"
						class="btn"
						@click="heal">{{ healText }}</button>

				<button
						id="give-up"
						class="btn"
						@click="giveUp">{{ giveUpText }}</button>
			</div>
		</section>
	</section>
</template>

<script>
	export default {
		name: 'SimpleGameLikeMortalCombat',
		data() {
			return {
				mainHeadingText: 'battle',
				nameOfFirstPlayer: 'you',
				nameOfSecondPlayer: 'monster',
				startGameText: 'start game',
				attackText: 'attack',
				specialAttackText: 'special attack',
				healText: 'heal',
				giveUpText: 'give up',
				healthOfFirstPlayer: 100,
				healthOfSecondPlayer: 100,
				gameIsRunning: false
			}
		},
		methods: {
			startGame() {
				this.gameIsRunning = true;
				this.healthOfFirstPlayer = 100;
				this.healthOfSecondPlayer = 100;
			},
			attack() {
				this.healthOfSecondPlayer -= this.calculateDamage(3, 12);

				if (this.healthOfFirstPlayer <=0) {
					this.checkWin();
					return;
				}
				
				this.secondPlayerAttack();
			},
			specialAttack() {
				this.healthOfSecondPlayer -= this.calculateDamage(10, 20);

				if (this.checkWin()) {
					return;
				}

				this.secondPlayerAttack();
			},
			heal() {
				if (this.healthOfFirstPlayer <= 90) {
					this.healthOfFirstPlayer += 10;
				} else {
					this.healthOfFirstPlayer = 100;
				}
				this.secondPlayerAttack();
			},
			giveUp() {
				this.gameIsRunning = false;
			},
			secondPlayerAttack() {
				this.healthOfFirstPlayer -= this.calculateDamage(5, 12);
				this.checkWin();
			},
			calculateDamage(min, max) {
				return Math.max(Math.floor(Math.random() * max) + 1, min);
			},
			checkWin() {
				if (this.healthOfFirstPlayer <= 0) {
					if (confirm('You lost! New game?')) {
						this.startGame();
					} else {
						this.gameIsRunning = false;
					}
					return true;
				} else if (this.healthOfSecondPlayer <= 0) {
					if (confirm('You won! New game?')) {
						this.startGame();
					} else {
						this.gameIsRunning = false;
					}
					return true;
				}

				return false;
			}
		}
	}
</script>

<style>
	.text-center {
		text-align: center;
	}

	.btn {
		display: inline-block;
		height: 50px;
		line-height: 50px;
		font-size: 24px;
		min-width: 50px;
		text-transform: uppercase;
		color: #651a57;
		cursor: pointer;
	}

	.btn + .btn {
		margin-left: 10px;
	}

	.main-section {
		width: 75%;
		max-width: 1005px;
		min-height: 100vh;
		margin-left: auto;
		margin-right: auto;
		padding: 10px;
		background-color: aliceblue;
	}

	.row {
		margin-bottom: 25px;
	}

	.column {
		display: inline-block;
		width: 47%;
	}

	.main-heading {
		margin-top: 15px;
		margin-bottom: 15px;
		font-size: 50px;
		color: #f55353;
		letter-spacing: 5px;
		text-transform: uppercase;
		text-shadow: 3px 1px 3px #737373;
	}

	.player-section + .player-section {
		margin-left: 3%;
	}

	.player-name {
		font-size: 32px;
		color: #d33658;
		letter-spacing: 3px;
		text-transform: uppercase;
		text-shadow: 3px 1px 3px #737373;
	}

	.healthbar {
		width: 100%;
		height: 70px;
		line-height: 70px;
		color: white;
		font-size: 24px;
		text-shadow: 1px 1px 3px black;
		background-color: #b9d8d8;
		transition: width 500ms;
	}

	#attack {
		color: #b9002e;
		background-color: aqua;
	}

	#special-attack {
		color: red;
		background-color: rgb(24, 22, 22);
	}

	#heal {
		color: #532719;
		background-color: orangered;
	}

	#give-up {
		color: white;
		background-color: black;
	}
</style>