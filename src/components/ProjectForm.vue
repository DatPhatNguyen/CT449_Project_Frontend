<template>
	<Form ref="projectForm" @submit="submitProject" :validation-schema="projectFormSchema">
		<div class="form-group " style="margin-bottom: 16px">
			<label for="name" style="font-weight:bold">Tên môn học:</label>
			<Field name="name" type="text" class="form-control" v-model="projectLocal.name"
				placeholder="Nhập vào tên môn học" />
			<ErrorMessage name="name" class="error-feedback" />
		</div>
		<div class="form-group " style="margin-bottom: 16px">
			<label for="decripsion" style="font-weight:bold">Mô tả:</label>
			<textarea v-model="projectLocal.decripsion" class="form-control"
				placeholder="Nhập vào mô tả của môn học"></textarea>
			<ErrorMessage name="decripsion" class="error-feedback" />
		</div>
		<div class="form-group " style="margin-bottom: 16px">
			<label for="link" style="font-weight:bold">Địa chỉ lưu trữ:</label>
			<Field name="link" type="text" class="form-control" v-model="projectLocal.link"
				placeholder="Nhập link lưu trữ của môn học" />
			<ErrorMessage name="link" class="error-feedback" />
		</div>

		<div class="form-group" style="margin-top: 16px;">
			<button class="btn btn-primary btn-block">
				<i class="fas fa-save"></i> Lưu
			</button>
			<button v-if="projectLocal._id" type="button" class="btn btn-danger btn-block " @click="deleteProject">
				<i class="fas fa-trash"></i> Xóa
			</button>
		</div>
	</Form>
</template>

<script>
import * as yup from "yup";
import { Form, Field, ErrorMessage } from "vee-validate";

export default {
	components: {
		Form,
		Field,
		ErrorMessage,
	},
	emits: ["submit:project", "delete:project"],
	props: {
		project: { type: Object, required: true },
		resetAfterSubmit: { type: Boolean, default: false },
	},
	data() {
		const projectFormSchema = yup.object().shape({
			name: yup
				.string()
				.required("Tên phải có giá trị.")
				.min(2, "Tên phải ít nhất 2 ký tự.")
				.max(50, "Tên có nhiều nhất 50 ký tự."),

			decripsion: yup.string().max(1000, "Mô tả tối đa 1000 ký tự."),
			link: yup.string(),

		});
		return {
			projectLocal: this.project,
			projectFormSchema,
		};
	},
	methods: {
		submitProject() {
			this.$emit("submit:project", this.projectLocal);
			if (this.resetAfterSubmit) {
				this.$refs.projectForm.resetForm();
			}
		},
		deleteProject() {
			this.$emit("delete:project", this.projectLocal._id);
		},
	},
};
</script>

<style scoped>
@import "@/assets/form.css";
</style>
