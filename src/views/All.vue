<template>
    <div id="All">
    	<el-container>
    		<el-header>
    			<el-row type="flex" class="row-bg" justify="space-around">
    				<el-col :span="6">
    					<div class="grid-content bg-purple name" >
							<i class="el-icon-shopping-cart-2" style="font-size: 30px; float: left; margin-top: 15px;"></i>
							<span style=" font-size: 30px;" class="title"><strong>{{isCollapse?'库存':'超市库存系统'}}</strong></span>
							<i class="el-icon-menu" size="mini" @click="handleMenu" style="font-size: 30px;  margin-top: 15px;margin-left: 10px;"></i>
						</div>
    				</el-col>
    				<el-col :span="6">
    					<div class="grid-content bg-purple-light" style="clear: both;"></div>
    				</el-col>
    				<el-col :span="12">
    					<div class="grid-content bg-purple">
    						<el-row class="demo-avatar demo-basic">
    							<div class="demo-basic--circle" style=" height: 60px; float: right;">
    								<el-avatar :size="50" :src="circleUrl" style="vertical-align: middle;margin-right: 0.625rem;">
    								</el-avatar>
    								<el-dropdown>
    									<el-button type="primary" round>
    										{{userName}}<i class="el-icon-arrow-down el-icon--right"></i>
    									</el-button>
    									<el-dropdown-menu slot="dropdown">
    										<el-dropdown-item @click.native="shutDown">注销</el-dropdown-item>
    									</el-dropdown-menu>
    								</el-dropdown>
    								
    							</div>
    						</el-row>
    					</div>
    				</el-col>
    			</el-row>
    		</el-header>
    		<el-container>
    			
				<el-menu class="el-menu-vertical-demo" background-color="#545c64"  text-color="#fff" @open="handleOpen" @close="handleClose" 
					active-text-color="#ffd04b" default-active="2-4-1" :collapse="isCollapse" router >
					<!-- :default-active="this.$route.path" -->
					<el-menu-item-group v-for="(v,i) in data1" :key="i">
						<el-menu-item  router="true"  v-for="(children,n) in v.children"  :index="children.path" :key="n" >
							<i :class="children.icon" style="margin-left:15px"></i>							
							<span>{{children.name}}</span>
						</el-menu-item>
					</el-menu-item-group>
				</el-menu>
    			
    			<el-main>
    				<router-view></router-view>
    			</el-main>
    		</el-container>
    	</el-container>
    </div>
</template>

<script>
    import defaultHeadPicture from "../assets/img/PI@8NE30H5Q(GAGMKB)XY@C.jpg"
	import http from '../util/http.js'
    export default {
        name: "All",
		data() {
			return {
				circleUrl: defaultHeadPicture,
				data1:[],
				userName:"",
				isCollapse:false
			}
		},
		mounted(){
			console.log('111',this.$router.options.routes)
			this.data1=this.$router.options.routes
		},
		methods:{
			handleOpen(key, keyPath) {
				console.log(key, keyPath);
			},
			handleClose(key, keyPath) {
				console.log(key, keyPath);
			},
			handleMenu(){
				this.isCollapse=!this.isCollapse
			},
			shutDown(){
				http({
					method:'get',
					url:'/logout'
				}).then((res)=>{
					console.log('side',res)
					if(res.data.code === 200){
						console.log(res)
						this.$router.push("/")
						window.localStorage.removeItem("userName")
					}else{
						alert("失败")
					}
				}).catch(()=>{
					console.log("注销失败")
				})
			}
		},
		created() {
			this.userName = localStorage.getItem("userName")
			console.log(this.userName)
		}
    }
</script>

<style scoped>
    .el-menu-vertical-demo:not(.el-menu--collapse) {
    width: 164px;
    min-height: 400px;
  }
    .name{
		display: flex;
	}
     .title{
		font-family:Georgia, 'Times New Roman', Times, serif;
		margin-left: 12px;
	}
	.el-header,
	.el-footer {
		background-color: #eff7fb;
		color: #333;
		text-align: center;
		line-height: 60px;
	}

	.el-aside {
		height: 792px;
		background-color: #545c64;
		color: #333;
		text-align: center;
		line-height: 200px;
		overflow: hidden;
	}

	

	body>.el-container {
		margin-bottom: 40px;
	}

	.el-container:nth-child(4) .el-aside {
		line-height: 320px;
	}
</style>