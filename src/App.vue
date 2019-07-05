<template>
  <div id="app">

<el-menu :default-active="activeIndex" class="el-menu-demo" mode="horizontal"background-color="rgb(84, 92, 100)" text-color="#fff">
	 <el-menu-item index="3">我的任务</el-menu-item>
	</el-menu>

	<el-container>
  <el-header><h3>添加任务：</h3></el-header>

</el-container>

<el-input v-model:value="value" v-on:keyup.native.13="keyup" placeholder="请输入内容"></el-input>
<el-row :gutter="20" style="margin:10px 0;border:none">
  <el-col :span="6"><div class="grid-content bg-purple"><el-tag type="danger">{{num}}个任务未完成</el-tag></div></el-col>
 <el-col :span="6" :offset="6"><div class="grid-content bg-purple">
 	<router-link to="/"><el-tag>所有任务</el-tag></router-link>
<router-link to="/unfinish"><el-tag type="warning">未完成任务</el-tag></router-link>
<router-link to="/finish"><el-tag type="success">完成任务</el-tag></router-link></div></el-col>
</el-row>

	<el-container>
  <el-header><h3>任务列表：</h3></el-header>
</el-container>

<el-row :gutter="20"  v-for="item in flagchecked">

  <el-col :span="6"><el-checkbox v-model:checked="item.checked"></el-checkbox></el-col>
  <el-col :span="12"><label @click="edit(item)" v-bind:class="{del:item.checked}">{{item.title}}</label></el-col>
  <el-col :span="6"><el-button type="danger" @click="dele(item)">删除</el-button></el-col>
  <el-col :span="12"><input type="text" name="" v-bind:class="{edit,editing:editing===item}" v-focus="editing===item" @keyup.13="ok" @keyup.esc="esc(item)" v-model="item.title" @blur="ok"></el-col>
</el-row>
  </div>
</template>

<script>
	var setLocal={
			save(key,value){
				localStorage.setItem(key,JSON.stringify(value))

			},
			get(key){
			return JSON.parse(localStorage.getItem(key))	
			}
		}
		var list=setLocal.get("do")||[];
		var flagcheck={
			all(list){
				return list
			},
			unfinish(list){
				console.log('unfinish')
				return list.filter(function(item){return item.checked==false})
			},
			finish(list){
				return list.filter(function(item){return item.checked==true})
			}

		}
export default {
  name: 'App',
      data() {
      return {
      	   checked: true,
      	 input: '',
        activeIndex: '1',
        activeIndex2: '1',
        list:list,
				value:"",
				editing:"",
				beforeedit:"",
				hash:'unfinish',
				path:'all',
				data:''
				
      }
    },
 
    watch:{
    			$route(to,from){
    				console.log(to.name)
    				this.path=to.name
    			},
				list:{
					deep:true,
					handler:function(){
					  	setLocal.save("do",this.list)
				}
			}
			},
			methods:{
				
				keyup(event){
					console.log(event)
					this.list.push({
						title:this.value,
						checked:false
					})	
					this.value="";
				},
						dele(item){
					var index=this.list.indexOf(item);
					this.list.splice(index,1);

				},
				edit(item){
					console.log('111')
					this.beforeedit=item.title
					this.editing=item;
				},
				ok(){
					this.editing="";
				},
				esc(item){
					item.title=this.beforeedit;
					this.editing=""
				}

		
			},
			directives:{
				focus:{
					update(el,binding){
						if(binding.value){
							el.focus();
						}
					}
				}
			},
		
			computed:{
				num(){
					return this.list.filter(function(item){
						return item.checked==false	
					}).length
				},
				flagchecked(){
					return flagcheck[this.path] ? flagcheck[this.path](this.list) :this.list
				}
			}
}
</script>

<style lang="less" scoped>

	#app{
		 .bg-purple-dark {
    background: #99a9bf;
  }
	.el-row{
		  border: 1px solid #ebebeb;
		display:flex;
		align-items:center;
	}
.edit{
	display: none;
	
}
.editing{
	display: inline-block;
	position: absolute;
	height: 100%;
	width: 600px;
	top:0;
}
.del{
	text-decoration:line-through


}
	}
</style>



git init
git add README.md
git add .
git commit -m "commit"
git remote add origin https://github.com/lisiyang52000/lists.git
git push -u origin master