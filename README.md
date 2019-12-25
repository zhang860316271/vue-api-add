<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
		<script src="js/vue.js" type="text/javascript" charset="utf-8"></script>
		<script src="https://cdn.staticfile.org/vue-resource/1.5.1/vue-resource.min.js" type="text/javascript" charset="utf-8"></script>
	</head>
	<body>
		<div id="app">
			<div class="panel panel-primary">
				<div class="panel-heading">
					<h3 class="panel-title">添加品牌</h3>
				</div>
			</div>
			<div class="panel-body form-inline">
				<label>
					Name:
					<input type="text" name="" id="" value="" class="form-control"  v-model="name"/>
				</label>
					<input type="button" name="" id="" value="添加" @click="add" class="btn btn-primary"/>
			</div>
			<table class="table table-bordered table-hover table-striped">
				<tr>
					<th>Id</th>
					<th>Name</th>
					<th>Ctime</th>
					<th>Operation</th>
					<th>操作</th>
				</tr>
				<tr v-for="item in list" :key="item.id">
					<td>{{item.id}}</td>
					<td>{{item.name}}</td>
					<td>{{item.ctome}}</td>
					<td>{{item.operation}}</td>
					<td><a href="" @click.prevent="del(item.id)">删除</a></td>
				</tr>
			</table>
		</div>
	</body>
	<script type="text/javascript">
		//如果我们通过全局配置了，应该以相对路径开头，前面不能带/ 否则不启用根路径做拼接
		Vue.http.options.root='http://www.liulongbin.top:3005/';
		//全局启用emulateJSON选项
		Vue.http.options.emulateJSON=true;
		window.onload=function(){
			var app=new Vue({
				el:'#app',
				data:{
					list:[//存放所有品牌列表的数组
						{id:1,name:'五菱宏光',ctime:new Date()},
						{id:2,name:'五菱宏光',ctime:new Date()}
					]
				},
				created(){//当vm实例的data和methods初始化完毕后，vm实例会自动化执行created
					this.getAllList()
					this.name=''
				},
				methods:{
					add(){//添加品牌列表到后台服务器
						//分析：
						//1.听过查看 数据API接口，发现，要发送一个Post请求，
						//2.this.$http.post()中接受三个参数
						//2.1 第一个参数：要请求的url地址
						//2.2 第二个参数：要提交服务器的数据，要以对象形式提交给服务器{name:this.name}
						//3.3 第三个参数：是一个配置对象，要以哪种表单数据类型提交过去，{emulateJSON:true}
						//3.在post方法中，使用.then来设置成功的回调函数，如果想要拿成功的结果，需要result.body
					
						this.$http.post('api/addproduct',{name:this.name}).then(result=>{
							if(result.body.status===0){
								this.getAllList()
							}else{
								alert("失败")
							}
					})
					},
					getAllList(){//获取所有车的品牌列表
						this.$http.get('api/getprodlist').then(result=>{
							var result=result.body
							if(result.status===0){
								this.list=result.message
								console.log("成功了")
							}else{
								alert("获取数据失败")
							}
						})
					},
					
					del(id){//删除品牌
						this.$http.get('api/delproduct/'+id).then(result=>{
							if(result.body.status===0){
								this.getAllList()
							}else{
								alert("删除失败")
							}
						})
					}
				},
				
			})
	}
	</script>
</html>
