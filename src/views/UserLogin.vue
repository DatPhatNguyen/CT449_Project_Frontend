<template>
	<div class="col-md-12 ">
		<div class="card card-container">
			<img id="profile-img"
				src="https://banner2.cleanpng.com/20180329/zue/kisspng-computer-icons-user-profile-person-5abd85306ff7f7.0592226715223698404586.jpg"
				alt="Cannot load the image" class="profile-img-card" />
			<Form @submit="handleLogin" :validation-schema="loginFormSchema">
				<div class="form-group" style="margin-top: 16px;">
					<label for="username" style="font-weight:bold">Tên đăng nhập:</label>
					<Field name="username" type="text" class="form-control" placeholder="Tên đăng nhập của bạn..." />
					<ErrorMessage name="username" class="error-feedback" />
				</div>
				<div class="form-group" style="margin-bottom: 16px;">
					<label for="password" style="font-weight:bold">Mật khẩu:</label>
					<Field name="password" type="password" class="form-control" placeholder="Mật khẩu của bạn..." />
					<ErrorMessage name="password" class="error-feedback" />
				</div>

				<div class="form-group">
					<button class="btn btn-primary btn-block" style="margin-top: 16px;" :disabled="loading">
						<span v-show="loading" class="spinner-border spinner-border-sm"></span>
						<span>Đăng Nhập</span>
					</button>
				</div>

				<div class="form-group">
					<div v-if="message" class="alert alert-danger" role="alert">
						{{ message }}
					</div>
				</div>
			</Form>
		</div>
	</div>
</template>

<script>
import { Form, Field, ErrorMessage } from "vee-validate";
import * as yup from "yup";
import { mapActions } from "pinia";
import { useAuthStore } from "@/stores/auth.store";

export default {
	components: {
		Form,
		Field,
		ErrorMessage,
	},
	data() {
		const loginFormSchema = yup.object().shape({
			username: yup.string().required("Tên đăng nhập phải có giá trị."),
			password: yup.string().required("Mật khẩu phải có giá trị."),
		});

		return {
			loading: false,
			message: "",
			loginFormSchema,
		};
	},
	methods: {
		...mapActions(useAuthStore, ["login"]),

		async handleLogin(user) {
			this.loading = true;

			try {
				await this.login(user);

				const redirectPath = this.$route.query.redirect || {
					name: "project",
				};

				this.$router.push(redirectPath);
			} catch (error) {
				console.log(error);

				this.loading = false;
				this.message = "Đã có lỗi xảy ra.";
			}
		},
	},
};
</script>

<style scoped>
@import "@/assets/form.css";
</style>
