<template>
 <div class="wrapper">
		<h1>Simon Says</h1>
			<div class="game-board">
				<div class="simon">
					<ul>
						<li @click.prevent="meth_click_red" :class="[red ? 'red' : 'tile red']" ></li>
						<li @click.prevent="meth_click_blue" :class="[blue ? 'blue' : 'tile blue']" ></li>
						<li @click.prevent="meth_click_yellow" :class="[yellow ? 'yellow' : 'tile yellow']" ></li>
						<li @click.prevent="meth_click_green" :class="[green ? 'green' : 'tile green']" ></li>
					</ul>
				</div>
			</div>
			<div class="game-info">
				<h2>Round: <span>{{round}}</span></h2>
				<button @click.prevent="start_round">Start</button>
				<p v-if="!going_to_win">Sorry, you lost</p>
			</div>
			<div class="game-options">
				<h2>Game Options:</h2>
				<input type="radio" name="mode" v-model="lvl" value="easy" checked>Easy<br>
				<input type="radio" name="mode" v-model="lvl" value="medium">Medium<br>
				<input type="radio" name="mode" v-model="lvl" value="hard">Hard<br>
			</div>
	</div>
</template>

<script>
export default {
  data () {
	return {
	  user_clicks: [],
	  gen_clicks: [],
	  lvl: 'easy',
	  time: 0,
	  chrono: null,
	  time_game: 0,
	  round: 0,
	  going_to_win: true,
	  comp: false,
	  red: false,
	  blue: false,
	  yellow: false,
	  green: false
    }
  },
  methods: {
	playSound (sound)
	{
      if(sound) {
		var audio = new Audio(require(sound))
		audio.append('<source src="' + sound + '" type="audio/mp3" />');
        audio.play();
	  }
	},
	start_round () 
	{
		console.log("Start")
		this.round = 0
		this.user_clicks = []
		this.chrono = null,
	  	this.time_game = 0,
		this.gen_clicks = []
		this.time_game = 0
		this.comp = false
		this.going_to_win = true
		switch (this.lvl)
		{
			case 'easy':
				this.time = 1500
				break;
			case 'medium':
				this.time = 1000
				break;
			case 'hard':
				this.time = 400
				break;
			default:
				break;
		}
		this.next_round()
	},
	next_round () 
	{
		console.log("next round")
		if(this.going_to_win)
		{
			this.comp = false
			this.user_clicks = []
			this.round += 1
			this.rand_gen()
			this.show_gen()
		}
	},
	start_play () 
	{
		setTimeout(() => this.playing(), 1500)
	},
	playing () 
	{
		setInterval(() =>  this.validation(), 100)
		//this.chrono = setInterval(this.validation(), 100)
		//console.log(chrono, this.going_to_win, this.time_game)
		console.log(this.user_clicks,this.gen_clicks)
	},
	validation ()
	{
		if(this.going_to_win)
		{
			this.isEqual()
			console.log("Compare? " + this.comp)
			if(this.comp)
			{
				this.next_round()
			}
			//console.log(this.time_game, this.time)
			if(this.time_game >= this.time)
			{
				this.time_game = 0
				this.going_to_win = false
				clearInterval(this.chrono)
			}
			this.time_game += 100
		}
	},	
	isEqual () 
	{
		if(this.user_clicks.length == this.gen_clicks.length)
		{	
			this.comp = true
			for (let i = 0; i < this.gen_clicks.length; i++)
			{	
				if(this.user_clicks[i] == this.gen_clicks[i])
				{
					console.log()
				}
				else{
					this.comp = false
					this.going_to_win = false
				}
			}
		}
	},
	async show_gen () 
	{
		setTimeout(() =>  this.show_color(), 500)
		await this.start_play()
	},
	show_color () 
	{
		var click_time = 0
		for (let i = 0; i < this.gen_clicks.length; i++)
		{
			const element = this.gen_clicks[i]
			switch (element)
			{
				case 1:
					setTimeout(() =>  this.meth_red(), click_time += 400)
					break;
				case 2:
					setTimeout(() =>  this.meth_blue(), click_time += 400)
					break;
				case 3:
					setTimeout(() =>  this.meth_yellow(), click_time += 400)
					break;
				case 4:
					setTimeout(() =>  this.meth_green(), click_time += 400)
					break;
			
				default:
					break;
			}
		}
	},
	rand_gen ()
	{
		this.gen_clicks.push(Math.floor((Math.random() * 4) + 1))
	},
	meth_red ()
	{
	  this.red = !this.red
	  setTimeout(() =>  this.red = !this.red, 150);
	  //this.playSound('../assets/sounds/1.mp3')
	},
	meth_blue ()
	{
	  this.blue = !this.blue
	  setTimeout(() =>  this.blue = !this.blue, 150);
	  //this.playSound('../assets/sounds/1.mp3')
	},
	meth_yellow () 
	{
	  this.yellow = !this.yellow
	  setTimeout(() =>  this.yellow = !this.yellow, 150);
	  //this.playSound('../assets/sounds/1.mp3')
	},
	meth_green () 
	{
	  this.green = !this.green
	  setTimeout(() =>  this.green = !this.green, 150);
	  //this.playSound('../assets/sounds/1.mp3')
	},
	meth_click_red () 
	{
	  this.time_game = 0	
	  this.meth_red()
	  this.user_clicks.push(1)
	},
	meth_click_blue () 
	{
	  this.time_game = 0
	  this.meth_blue()
	  this.user_clicks.push(2)
	},
	meth_click_yellow () 
	{
	  this.time_game = 0
	  this.meth_yellow()
	  this.user_clicks.push(3)
	},
	meth_click_green () 
	{
	  this.time_game = 0
	  this.meth_green()
	  this.user_clicks.push(4)
    },
  }
}
</script>
