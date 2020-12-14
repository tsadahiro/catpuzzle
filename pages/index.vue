<template>
<div>
  <v-btn @click="show=true">show</v-btn>
  <svg v-if="show" width="900" height="900">
    <g v-for="(row,i) in pieces">
      <piece v-for="(p,j) in row":seq="p" N="3" :row="i" :col="j"></piece>
    </g>
  </svg>
</div>
</template>

<script>
import puzzleboard from '~/components/puzzleboard'
import piece from '~/components/piece'

export default {
    data(){
	return {
	    seq: [0,1,0, 1,1,0, 1,1,0, 0,1,0],
	    //seq: [0,1, 1,1, 0,0, 1,0],
	    pieces: [
		//[
		//    [0,1, 1,0, 0,0, 1,1],[1,1, 0,0, 0,1, 0,1]
		//],
		//[
		//    [0,0, 1,1, 0,1, 0,1], [1,0, 0,1, 0,0, 1,1] 
		//]
		//],
		//[
		//    [0,1,1,1,0,0,0,1], [1,1,0,0,0,0,1,1], [0,1,1,1,0,1,0,0]
		//],
		//[
		//    [0,0,1,1,0,1,0,1], [0,0,1,0,0,1,1,1], [1,0,0,1,0,1,0,1]
		//]
		[
		    [1,1,1,0,1,0,1,0,0,0,0,1], [1,0,1,1,1,0,1,0,0,0,1,0], [0,0,0,1,0,0,1,1,1,0,1,1] 
		],
		[
		    [0,0,1,1,1,1,1,0,0,1,0,0], [0,0,1,0,0,1,0,1,0,1,1,1], [1,1,1,0,1,0,1,0,0,1,0,0] 
		],
		[
		    [0,0,1,1,1,1,0,0,0,0,1,1], [0,1,0,0,1,0,1,0,0,1,1,1], [0,0,1,0,1,1,1,0,1,0,1,0] 
		]
	    ],
	    show: false,
	}
    },
    components: {
	puzzleboard,
	piece
  }
}
</script>
