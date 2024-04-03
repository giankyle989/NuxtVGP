<template>
	<v-container>
		<h2>
			<v-icon icon="mdi-vuetify" />
			Starter Template
		</h2>
		<h5>Nuxt 3 / Vuetify / Graphql / Pinia</h5>

		<h3 class="my-5">
			Example Vuetify
			<v-chip color="blue">SimpleTable</v-chip>
			<v-chip color="orange">Data from spaceX graphql</v-chip>
		</h3>
		<p>There are {{ launches?.length || 0 }} launches.</p>
		<v-table>
			<thead>
				<tr>
					<th class="text-left">Mission Name</th>
					<th class="text-left">Launch Date</th>
					<th class="text-left">Launch Site Name</th>
					<th class="text-left">Rocket Name</th>
					<th class="text-left">Details</th>
				</tr>
			</thead>
			<tbody>
				<tr v-for="launch in launches" :key="launch.id">
					<td>{{ launch?.mission_name }}</td>
					<td>{{ launch?.launch_date_utc }}</td>
					<td>{{ launch?.launch_site?.site_name }}</td>
					<td>{{ launch?.rocket?.rocket_name }}</td>
					<td>{{ launch?.details }}</td>
				</tr>
			</tbody>
		</v-table>
	</v-container>
</template>
<script lang="ts" setup>
const selection = ref(0)
const query = gql`
	query getLaunches {
		launches {
			id
			mission_name
			launch_date_utc
			launch_site {
				site_name
			}
			rocket {
				rocket_name
			}
			details
		}
	}
`
const { data } = useAsyncQuery<{
	launches: {
		id: String
		mission_name: String
		launch_date_utc: Date
		launch_site: {
			site_name: String
		}
		rocket: {
			rocket_name: String
		}
		details: String
	}[]
}>(query)
const launches = computed(() => data.value?.launches ?? [])
</script>
