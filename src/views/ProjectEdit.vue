<template>
	<div v-if="project" class="page border border-1 p-3 mt-5">
		<h4 class="text-center text-capitalize">Chỉnh sửa môn học</h4>
		<ProjectForm :project="project" @submit:project="updateProject" @delete:project="deleteProject" />
	</div>
</template>

<script>
import { swtoast, swalert } from "@/mixins/swal.mixin";
import ProjectForm from "@/components/ProjectForm.vue";
import ProjectService from "@/services/project.service";

export default {
	components: {
		ProjectForm,
	},
	props: {
		id: { type: String, required: true },
	},
	data() {
		return {
			project: null,
		};
	},
	methods: {
		async getProject(id) {
			try {
				this.project = await ProjectService.get(id);
			} catch (error) {
				console.log(error);
				this.$router.push({
					name: "notfound",
					params: { pathMatch: this.$route.path.split("/").slice(1) },
					query: this.$route.query,
					hash: this.$route.hash,
				});
			}
		},

		async updateProject(data) {
			try {
				await ProjectService.update(this.project._id, data);
				swtoast.success({
					text: "Môn học được cập nhật thành công.",
				});
				this.$router.push({ name: "project" });
			} catch (error) {
				console.log(error);
				swtoast.error({
					text: "Đã có lỗi xảy ra.",
				});
			}
		},

		async deleteProject() {
			swalert
				.fire({
					title: "Xóa môn học",
					icon: "warning",
					text: "Bạn muốn xóa môn học này?",
					showCloseButton: true,
					showCancelButton: true,
				})
				.then(async (result) => {
					if (result.isConfirmed) {
						try {
							await ProjectService.delete(this.project._id);
							swtoast.success({
								text: "Môn học được xóa thành công.",
							});
							this.$router.push({ name: "project" });
						} catch (error) {
							console.log(error);
							swtoast.error({
								text: "Đã có lỗi xảy ra.",
							});
						}
					}
				});
		},
	},
	created() {
		this.getProject(this.id);
	},
};
</script>
