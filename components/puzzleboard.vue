<template>
<div>
  <div>
    <v-btn v-if="isLoaded">solve</v-btn>
    <v-file-input multiple label="画像を選択" @change="drawCanvas($event)"></v-file-input>

    <v-card>
      <v-row>
	<v-col>
	  <canvas id="canvas" height="600" width="600" style="background-color:gray;"></canvas><br/>
	</v-col>
	<v-col>
	  <v-row v-for="(r,i) in borders">
	    <v-row v-for="(c,j) in r">
	      <v-col>
		{{(1*i+1)}}-{{(1*j+1)}}:
		<v-text-field v-model="c[0]"></v-text-field>
		<v-text-field v-model="c[1]"></v-text-field>
		<v-text-field v-model="c[2]"></v-text-field>
		<v-text-field v-model="c[3]"></v-text-field>
	      </v-col>
	    </v-row>
	  </v-row>
	</v-col>
      </v-row>
    </v-card>
	

    <div id="result"></div>
  </div>  
  <svg width="600" height="600"></svg>
</div>
</template>

<script>

export default {
    data(){
	return{
	    borders:[
		[
		    ["0,1,0,0,1","0,0,1,0,1","1,0,1,0,0","1,1,0,1,0",],
		    ["0,1,0,1,1","0,1,1,0,1","1,0,0,1,0","1,0,1,0,0",],
		    ["0,1,0,1,1","0,0,1,0,1","1,0,1,1,0","1,0,0,1,0",]
		],
		[
		    ["0,0,1,0,1","0,1,0,0,1","1,0,1,1,0","1,1,0,1,0",],
		    ["0,0,1,0,1","1,1,0,1,0","0,1,1,0,1","1,0,0,1,0",],
		    ["1,0,0,1,0","0,1,1,0,1","0,1,0,1,1","1,0,1,0,0",]
		],
		[
		    ["1,0,1,0,0","1,1,0,1,0","0,1,1,0,1","0,1,0,0,1",],
		    ["1,0,0,1,0","1,0,1,0,0","0,1,1,0,1","0,1,0,1,1",],
		    ["1,0,0,1,0","0,1,0,0,1","0,1,0,1,1","1,0,1,0,0",]
		],
	    ],
	    originalImage: null,
	    isLoaded:false,
	}
    },
    watch:{
	borders(){
	    console.log("watcher")
            let canvas = document.getElementById('canvas');
            let canvasWidth = 600;
            let canvasHeight = 600;
            canvas.width = canvasWidth;
            canvas.height = canvasHeight;
            let ctx = canvas.getContext('2d');
	    ctx.putImageData(this.originalImage,0,0)
	    const pal = ["yellow","#A3682A"]
	    for (let r in this.borders){
	    	for (let c in this.borders[r]){
	    	    let pos = -0.1
	    	    for (let col of this.borders[r][c][0].split(",")){
	    		pos+=0.2
	    		ctx.fillStyle=pal[1*col]
	    		ctx.fillRect((1*c+pos)*200-5, r*200, 10,10) 
	    	    }
	    	    pos = -0.1
	    	    for (let col of this.borders[r][c][1].split(",")){
	    		pos+=0.2
	    		ctx.fillStyle=pal[1*col]
	    		ctx.fillRect((1*c+1)*200-10, (1*r+pos)*200-5, 10,10) 
	    	    }
	    	    pos = 1.1
	    	    for (let col of this.borders[r][c][2].split(",")){
	    		pos-=0.2
	    		ctx.fillStyle=pal[1*col]
	    		ctx.fillRect((1*c+pos)*200-5, (1*r+1)*200-10, 10,10) 
	    	    }
	    	    pos = 1.1
	    	    for (let col of this.borders[r][c][3].split(",")){
	    		pos-=0.2
	    		ctx.fillStyle=pal[1*col]
	    		ctx.fillRect((1*c)*200, (1*r+pos)*200-5, 10,10) 
	    	    }
	    	}
	    }
	}
    },
    methods: {
        drawCanvas(e) {
	    console.log(e[0])
            let fileData = e[0]
            if (!fileData.type.match('image.*')) {
                alert('画像を選択してください')
                return
            }
            let canvas = document.getElementById('canvas');
            let canvasWidth = 600;
            let canvasHeight = 600;
            canvas.width = canvasWidth;
            canvas.height = canvasHeight;
            let ctx = canvas.getContext('2d');

            let reader = new FileReader();
            let that = this;
            reader.onload = function() {
                let uploadImgSrc = reader.result;
                let img = new Image();
                img.src = uploadImgSrc;
                img.onload = function() {
		    ctx.drawImage(img, 0, 0, img.width, img.height, 0,0,600,600)
		    for (let i of [1,2]){
			ctx.beginPath()
			ctx.moveTo(200*i,0)
			ctx.lineTo(200*i,900)
			ctx.closePath()
			ctx.strokeStyle="red"
			ctx.lineWidth=3
			ctx.stroke()

			ctx.beginPath()
			ctx.moveTo(0,200*i)
			ctx.lineTo(900,200*i)
			ctx.closePath()
			ctx.strokeStyle="red"
			ctx.lineWidth=3
			ctx.stroke()
		    }
		    that.originalImage = ctx.getImageData( 0, 0, canvas.width, canvas.height)
		    const pal = ["yellow","#A3682A"]
		    for (let r in that.borders){
			for (let c in that.borders[r]){
			    let pos = -0.1
			    for (let col of that.borders[r][c][0].split(",")){
				console.log(c,r,pal[1*col])
				pos+=0.2
				ctx.fillStyle=pal[1*col]
				ctx.fillRect((1*c+pos)*200-5, r*200, 10,10) 
			    }
			    pos = -0.1
			    for (let col of that.borders[r][c][1].split(",")){
				console.log(c,r,pal[1*col])
				pos+=0.2
				ctx.fillStyle=pal[1*col]
				ctx.fillRect((1*c+1)*200-10, (1*r+pos)*200-5, 10,10) 
			    }
			    pos = 1.1
			    for (let col of that.borders[r][c][2].split(",")){
				console.log(c,r,pal[1*col])
				pos-=0.2
				ctx.fillStyle=pal[1*col]
				ctx.fillRect((1*c+pos)*200-5, (1*r+1)*200-10, 10,10) 
			    }
			    pos = 1.1
			    for (let col of that.borders[r][c][3].split(",")){
				console.log(c,r,pal[1*col])
				pos-=0.2
				ctx.fillStyle=pal[1*col]
				ctx.fillRect((1*c)*200, (1*r+pos)*200-5, 10,10) 
			    }
			}
		    }
		    
                }
            }
            reader.readAsDataURL(fileData);
	    this.isLoaded = true
        }
    }
}
</script>
