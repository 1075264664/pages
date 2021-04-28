<template>
 <div class="nav">
 	  <el-row class='str'>
       <el-col :span='4' class='left' @click.native='Return'>
       	  <i class="fa fa-angle-left fa-2x" aria-hidden="true"></i>
       	  <span>返回</span>
       </el-col>
        <el-col :span='14' class='book'>书籍目录</el-col>
         <el-col :span="3" class='sj'><i class="el-icon-s-data"></i></el-col>
        <el-col :span='3' class='icon'><i class="el-icon-s-home"></i></el-col>
    </el-row>

    
    
   	<div class="md">
  		 <p @click='getState(item,{name:"Page",params:{title: item.link }},index)' v-for="(item,index) in list" :key='index'>{{item.title}}</p>
  	</div>
  
 </div>
</template>

<script>
	import BScroll from 'better-scroll'
	export default{
		name:'Directory',
		data(){
			return{
				list:[]
			}
		},
		created(){
			
			var id = JSON.parse(localStorage.getItem('id'))
			
		  this.axios.get('api/mix-atoc/'+ id +'?view=chapters').then(r =>{
		  	console.log('r',r)
		  	
		  	 this.list = r.data.mixToc.chapters
		  })
		},
		mounted(){
	        
		},
		methods:{
	     Return(){
			this.$router.go(-1)
		   },
			getState(item,path,index){
			
				localStorage.setItem('index',JSON.stringify(index))
				localStorage.setItem('Chapter',JSON.stringify(item.title))
				
				var id = JSON.parse(localStorage.getItem('id'))
				var a = JSON.parse(localStorage.getItem('getlist'))
				
				for(var i=0;i<a.length;i++){
					if(a[i]._id == id){
						
					    a[i].index = index
					    a[i].link =this.list[index].link 
					     a[i].name =this.list[index].title 
					   
					   localStorage.setItem('getlist',JSON.stringify(a))
					}
				}
				
				
				this.$router.push(path)
			},
			add(item,path){
				
				localStorage.setItem('link',JSON.stringify(item.link))
				
				
				this.$router.push(path)
			}
	
		}
	}
</script>

<style lang="less" scoped>
	.str{
		width: 100%;
		background-color:#FF0000;
		.left{
			height: 50px;
			text-align:right ;
			i{
				font-size: 0.6rem;
				float: left;
				line-height: 50px;
			   margin-left: 8px;
			   color: #FFFFFF;
			}
			span{
				font-size: 0.25rem;
				float: left;
				line-height: 50px;
				margin-left: 3px;
				color: #FFFFFF;
			}
		}
		.book{
			font-size: 0.25rem;
			font-weight: bold;
			text-align: center;
			line-height: 50px;
			color: #FFFFFF;
		}
		.sj{
			font-size: 0.3rem;
			text-align: center;
			line-height: 50px;
			i{
				color: #FFFFFF;
			}
		}
		.icon{
			font-size: 0.3rem;
			text-align: center;
			line-height: 50px;
			i{
				color: #FFFFFF;
			}
		}
	}
  .md{
		width: 100%;
		height: 736px;
		background: #FFFFFF;
		p{
			width: 100%;
			height:30px;
			line-height: 30px;
			margin: 3px;
			font-size: 0.3rem;
			border-bottom: 1px solid #333333;
			
		}
	}
</style>