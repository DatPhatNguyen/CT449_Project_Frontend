<template>
	<div class="page">
		<div class="border border-1 p-3 mt-5">
			<h4 class="text-center p-3 text-capitalize">Thêm môn học mới</h4>
			<ProjectForm :project="{}" :resetAfterSubmit="false" @submit:project="saveProject" />
		</div>
	</div>
</template>

<script>
import { swtoast } from "@/mixins/swal.mixin";
import ProjectForm from "@/components/ProjectForm.vue";
import ProjectService from "@/services/project.service";

export default {
	components: {
		ProjectForm,
	},
	methods: {
		async saveProject(data) {
			try {
				await ProjectService.create(data);
				swtoast.success({
					text: "Môn học được thêm thành công.",
				});
				this.$router.push({ name: "project" });
			} catch (error) {
				console.log(error);
				swtoast.error({
					text: "Đã có lỗi xảy ra.",
				});
			}
		},
	},
};
</script>
