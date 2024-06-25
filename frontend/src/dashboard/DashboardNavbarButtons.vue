<script setup>
import { useMagicKeys, whenever } from '@vueuse/core'
import { inject } from 'vue'
import DashboardMenuButton from './DashboardMenuButton.vue'
import DashboardShareButton from './DashboardShareButton.vue'
import sessionStore from '@/stores/sessionStore';
const dashboard = inject('dashboard')
const session = sessionStore()

const keys = useMagicKeys()
const cmdE = keys['Meta+E']
whenever(cmdE, dashboard.edit)
const cmdS = keys['Meta+S']
whenever(cmdS, dashboard.save)
const cmdD = keys['Meta+D']
whenever(cmdD, dashboard.discardChanges)
</script>

<template>
	<div class="flex flex-shrink-0 justify-end space-x-2">
		<DashboardShareButton v-if="!dashboard.editing && dashboard.canShare" />
		<Button variant="outline" v-if="!dashboard.editing" @click="dashboard.refresh">
			Refresh
		</Button>
		<Button v-if="dashboard.editing" variant="outline" @click="dashboard.discardChanges">
			Cancel
		</Button>
		<div v-if="session.user.is_user && session.user.system_user !=='no'">
			<Button v-if="!dashboard.editing" variant="outline" @click="dashboard.edit"> Edit </Button>
		<Button v-else  variant="solid" @click="dashboard.save" :loading="dashboard.loading">
			Save
		</Button>
		</div>
		<DashboardMenuButton />
	</div>
</template>
