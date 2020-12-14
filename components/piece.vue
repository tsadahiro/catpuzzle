<template>
<g :transform="position()">
  <rect x="0" y="0" :width="N*unit" :height="N*unit" fill="pink" stroke="black"></rect>
  <g v-for="(z,i) in zeroPair">
     <path  :d="curve(z)" stroke="yellow" stroke-width="8" fill="transparent"></path>
     <path :d="curve(onePair[i])" stroke="brown" stroke-width="8" fill="transparent"></path>
  </g>
</g>
</template>

<script>
export default{
    props:['seq','N','row','col'],
    data(){
	return {
	    zeroPair:null,
	    onePair:null,
	    unit:100
	}
    },
    created(){
	const pairs = this.randomMatch();
	this.zeroPair = pairs.zeroPair
	this.onePair = pairs.onePair
	console.log(this.zeroPair)
    },
    methods:{
	coordinate(k){
	    k=1*k
	    const unit = 100;
	    if (k < this.N){ //北
		return {x: (k*1+0.5)*unit, y:0}
	    }
	    if (k >= this.N && k < 2*this.N){//東
		return {x: (this.N*1)*unit, y: (k*1-this.N+0.5)*unit}
	    }
	    if (k >= 2*this.N && k < 3*this.N){//南
		//console.log({x: (3*this.N-1*k)*unit, y: 4*unit})
		return {x: (3*this.N-1*k-0.5)*unit, y: (1*this.N)*unit}
	    }
	    if (k >= 3*this.N ){//西
		console.log(k)
		console.log({x: 0, y: (4*this.N-1*k)*unit})
		return {x: 0, y: (4*this.N-1*k-0.5)*unit}
	    }
	},
	position(){
	    let str = "translate("
	    str += (this.col*this.unit*(1*this.N)) + ","
	    str += (this.row*this.unit*(1*this.N)) + ")"
	    return str
	},
	curve(pair){
	    const unit = 100
	    //let mx = Math.round(0.25*unit+Math.random()*unit*0.5);
	    let mx = Math.round((0.2+Math.random()*0.6)*this.N*unit);
	    let my = Math.round((0.2+Math.random()*0.6)*this.N*unit);
	    let path = "M " + this.coordinate(pair[0]).x + " " + this.coordinate(pair[0]).y;
	    path += " C " + mx + " " + my + ", "
	    mx = Math.round((0.2+Math.random()*0.6)*this.N*unit);
	    my = Math.round((0.2+Math.random()*0.6)*this.N*unit);
	    path += mx + " " + my + ", "
	    path += this.coordinate(pair[1]).x + " " + this.coordinate(pair[1]).y;
	    return path;
	},
	randomMatch(){
	    const zero=[]
	    const one=[]
	    for (let i in this.seq){
		if (this.seq[i]==0){
		    zero.push(i);
		}
		else{
		    one.push(i);
		}
	    }
	    const zeroPair=[]
	    const onePair=[]
	    while (zero.length > 0){
		let r = Math.floor(Math.random()*(zero.length-1))+1
		zeroPair.push([zero[0],zero[r]]);
		zero.splice(r,1);
		zero.splice(0,1);
	    }
	    while (one.length > 0){
		let r = Math.floor(Math.random()*(one.length-1))+1
		onePair.push([one[0],one[r]]);
		one.splice(r,1);
		one.splice(0,1);
	    }
	    return {zeroPair:zeroPair, onePair:onePair}
	}
    }
}
</script>
