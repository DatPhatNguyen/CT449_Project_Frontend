<template>
	<nav class="navbar navbar-expand navbar-dark bg-primary py-3 d-flex ">
		<div><a href="/" class="navbar-brand">Ứng dụng quản lý dự án</a></div>
		<div v-if="currentUser" class="navbar-nav">
			<div class="d-flex">
				<li class="nav-item">
					<router-link :to="{ name: 'project' }" class="nav-link text-white">
						Quản lý môn học
						<i class="fa-solid fa-clipboard-list"></i>
					</router-link>
				</li>
				<li class="nav-item">
					<router-link :to="{ name: 'deadline' }" class="nav-link text-white">
						Quản lý deadline
						<i class="fa-solid fa-calendar"></i>
					</router-link>
				</li>
				<li class="nav-item">
					<router-link :to="{ name: 'status' }" class="nav-link text-white">
						Tình trạng deadline
						<i class="fa-solid fa-bars-progress"></i>
					</router-link>
				</li>
			</div>
		</div>


		<div v-if="!currentUser" class="navbar-nav">
			<li class="nav-item  ">
				<router-link :to="{ name: 'login' }" class="nav-link">Đăng nhập</router-link>
			</li>
			<li class="nav-item ">
				<router-link :to="{ name: 'register' }" class="nav-link">Đăng ký</router-link>
			</li>

		</div>

		<div v-if="currentUser" class="navbar-nav ml-auto align-items-center">
			<li class="nav-item">
				<router-link :to="{ name: 'profile' }" class="nav-link text-white"><span>Xin chào, </span>{{
					currentUser.username
				}}</router-link>
			</li>
			<li class="nav-item">
				<a class="nav-link text-white" @click.prevent="handleLogout"><button class="btn btn-warning">Đăng
						Xuất</button></a>
			</li>
		</div>
	</nav>
</template>

<script>
import { mapState, mapActions } from "pinia";
import { useAuthStore } from "@/stores/auth.store";

export default {
	computed: {
		...mapState(useAuthStore, {
			currentUser: "user",
		}),
	},
	methods: {
		...mapActions(useAuthStore, ["logout", "loadAuthState"]),

		handleLogout() {
			this.logout();
			this.$router.push({ name: "login" });
		},
	},
	created() {
		this.loadAuthState();
	},
};
</script>
