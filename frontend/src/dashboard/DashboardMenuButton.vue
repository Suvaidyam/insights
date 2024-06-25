<script setup>
import sessionStore from '@/stores/sessionStore';
import { downloadImage } from '@/utils'
import { inject, ref } from 'vue'
import { useRouter } from 'vue-router'

const dashboard = inject('dashboard')
const showDeleteDialog = ref(false)
const session = sessionStore()

const router = useRouter()
async function handleDelete() {
	await dashboard.deleteDashboard()
	showDeleteDialog.value = false
	router.push({ name: 'Dashboards' })
}

async function downloadDashboardImage() {
	const $el = document.querySelector('.dashboard')
	await downloadImage($el, `${dashboard.doc.title}.png`)
}

</script>

<template>
	<Dropdown
		v-if="dashboard.doc"
		placement="left"
		:button="{ icon: 'more-vertical', variant: 'outline' }"
		:options="[
			{
				label: 'Export as PNG',
				variant: 'outline',
				icon: 'download',
				onClick: () => downloadDashboardImage(),
			},
			session.user.is_user && session.user.system_user !== 'no' ?
			{
				label: 'Delete',
				variant: 'outline',
				icon: 'trash-2',
				onClick: () => (showDeleteDialog = true),
			}
			:null
		]"
	/>

	<Dialog
		v-model="showDeleteDialog"
		:dismissable="true"
		:options="{
			title: 'Delete Dashboard',
			message: 'Are you sure you want to delete this dashboard?',
			icon: { name: 'trash', variant: 'solid', theme: 'red' },
			actions: [
				{ label: 'Cancel', variant: 'outline', onClick: () => (showDeleteDialog = false) },
				{ label: 'Yes', variant: 'solid', theme: 'red', onClick: handleDelete },
			],
		}"
	>
	</Dialog>
</template>
