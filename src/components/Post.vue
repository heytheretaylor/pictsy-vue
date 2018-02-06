<template>
  <div class="post">
	  <!-- {{post.title}} -->
	  	
		<v-tooltip @click.native.stop="dialog = true" bottom>
	  		<img class="cover" slot="activator" :src="coverURL">
			<span>{{post.title}}</span>
		</v-tooltip>
		<v-dialog class="popup" v-model="dialog">
			<v-card dark v-bind:style="styleObject">
				<v-icon @click="dialog = false"  @keydown.esc="closer" class="close">close</v-icon>
				<h2>{{post.title}}</h2>
				<div class="tag-holder">
					<v-chip v-for="tag in post.tags" :key="tag.ups">{{tag.name}}</v-chip>
				</div>
				<div class="image-holder">
					<img class="image" v-for="image in post.images" v-bind:key="image.id" :src="image.link">
				</div>
				<v-list class="list" two-line dark>
					<template v-for="(comment, key, index) in comments">
						<v-card style="margin: 10px" v-bind:key="index" color="green">
							<v-card-text v-html="comment.body"></v-card-text>
							<v-list-tile-sub-title v-html="comment.date"></v-list-tile-sub-title>
						</v-card>
					</template>
				</v-list>
				<div class="title-input">
					<v-text-field
						name="input-1"
						label="Enter a Comment"
						v-model="comment"
						@keyup.enter="submit"
					></v-text-field>
				</div>
			</v-card>
		</v-dialog>
  </div>
</template>

<script>
export default {
  name: 'Post',
  props: 
	  ["post"]
  ,
  data () {
    return {
	  dialog: false,
	  styleObject: {
		  display: "grid",
		  justifyContent: "center",
		  gridTemplateColumns: "[first] 728px [image] 300px [end]",
		  gridTemplateRows: "[top] 100px [head1] 75px [comment] auto [content] 20px [footer]",
		  gridGap: "20px"
	  },
	  comments:[],
	  comment:""
    }
  },
  computed: {
	  coverURL: function() {
		  let coverID = ''
		  if(this.post.cover){
			  coverID = this.post.cover
		  } else {
			  coverID = this.post.id
		  }
			  return `https://i.imgur.com/${coverID}b.jpg`		  
		  
		
	  }
  },
  methods: {
	  submit: function(){
		  let date = new Date;
		  date = date.toString();
		  let obj = {
			  body: this.comment,
			  date: date
		  }
		  this.comments.push(obj);
		  this.comment = ""
	  },
	  closer: function(){
		  console.log("Should be closing")
	  }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
 .cover{
	 height: 180px;
	 width: 180px;
	 border: 2px solid rgb(68, 68, 66);
 }
 .post {
	
	 align-items: center;
	 justify-content: center;
	 
 }

h2{
	align-self: center
}

.image{

	max-width: 728px;
	justify-self: center;
}
.image-holder{
	grid-column-start: first;
	grid-row-start: head1;
	grid-row-end: span content;
}

.title-input{
	grid-column: image;
	grid-row: head1;
}

.list{
	grid-column-start: image;
	grid-row-start: comment;
}

.close{
	position: absolute;
	left: 30px;
	top: 30px;
	cursor: pointer;
}
.tag-holder{
	margin-top: 10px;
}
 
</style>
