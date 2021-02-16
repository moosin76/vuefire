<template>
	<v-app-bar-title>
		{{ title }}
		<template>
			
			<v-btn icon @click="openDialog">
				<v-icon>mdi-pencil-outline</v-icon>
			</v-btn>
			
			<v-dialog v-model="dialog" max-width="400" :retain-focus="false">
				<v-card>
					<v-card-title>
						타이틀 수정
						<v-spacer />
						<v-btn icon @click="dialog = false">
							<v-icon>mdi-close</v-icon>
						</v-btn>
					</v-card-title>

					<v-card-text>
						<v-text-field
							v-model="text"
							label="제목 수정"
							append-outer-icon="mdi-pencil-outline"
							@click:append-outer="save"
							@keypress.enter="save"
						>
						</v-text-field>
					</v-card-text>
				</v-card>
			</v-dialog>
		</template>
	</v-app-bar-title>
</template>

<script>
export default {
	props: ["title"],
	data: () => ({
		dialog: false,
		text: "",
	}),
	methods: {
		openDialog() {
			this.text = this.title;
			this.dialog = true;
		},
		save() {
			this.$firebase
				.database()
				.ref("site")
				.update({ title: this.text }, (err) => {
					if (err) {
						console.log(err.message);
					} else {
						this.dialog = false;
					}
				});
		},
	},
};
</script>

<style>
</style>