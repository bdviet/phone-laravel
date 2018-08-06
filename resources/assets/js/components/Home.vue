<template>
	<div>
		<nav class="panel column is-offset-2 is-8">
			<p class="panel-heading">
				Danh bạ
				<!-- <button class="button is-link" > -->
					<a class="button is-link" @click="openAdd">Thêm mới</a>
				<!-- </button> -->
			</p>
			<div class="panel-block">
				<p class="control has-icons-left">
					<input class="input is-small" type="text" placeholder="search">
					<span class="icon is-small is-left">
						<i class="fas fa-search" aria-hidden="true"></i>
					</span>
				</p>
			</div>
			<a class="panel-block" v-for="item,key in lists">
				<span class="column is-9">
					{{ item.name }}
				</span>
				<span class="has-text-primary panel-icon column is-1">
					<i class="fa fa-eye" aria-hidden="true" @click="openShow(key)"></i>
				</span>
				<span class="has-text-info panel-icon column is-1">
					<i class="fa fa-edit" aria-hidden="true" @click="openUpdate(key)"></i>
				</span>
				<span class="has-text-danger panel-icon column is-1">
					<i class="fa fa-trash" aria-hidden="true"></i>
				</span>
			</a>

		</nav>

		<!-- components -->
		<Add :openmodal='addActive' @closeRequest='close'></Add>
		<Show :openmodal='showActive' @closeRequest='close'></Show>
		<Update :openmodal='updateActive' @closeRequest='close'></Update>

	</div>

</template>

<script>
let Add = require('./Add.vue');
let Show = require('./Show.vue');
let Update = require('./Update.vue');	
	export default{

		components:{Add,Show,Update},

		data(){
			return{

				addActive : '',
				showActive : '',
				updateActive : '',
				lists:{},
				errors:{}

			}
		},

		mounted(){
			axios.post('/getData')
			.then((response)=> this.lists = response.data)
			.catch((error) => this.errors = error.response.data.errors)
		},

		methods:{
			openAdd(){
				this.addActive = 'is-active';
			},
			openShow(key){
				this.$children[1].list = this.lists[key]
				this.showActive = 'is-active';
			},
			openUpdate(key){
				this.$children[2].list = this.lists[key]
				this.updateActive = 'is-active';
			},
			close(){
				this.addActive = this.showActive = this.updateActive = ''
			}
			
		}
	}
</script>