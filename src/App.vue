<template>
	<v-app>
		<v-navigation-drawer app v-model="drawer">
			<site-drawer :items="site.menu" />
		</v-navigation-drawer>

		<v-app-bar app color="primary" dark>
			<v-app-bar-nav-icon @click="drawer = !drawer" />
			<site-title :title="site.title" />
			<v-spacer />
			<v-btn icon @click="readOnce">
				<v-icon>mdi-check-all</v-icon>
			</v-btn>
			<v-btn icon @click="read">
				<v-icon>mdi-check</v-icon>
			</v-btn>
			<v-btn icon @click="save">
				<v-icon>mdi-pencil-outline</v-icon>
			</v-btn>
			<v-btn icon to="/about"><v-icon>mdi-magnify</v-icon></v-btn>
			<v-btn icon to="/"><v-icon>mdi-home</v-icon></v-btn>
		</v-app-bar>

		<v-main>
			<router-view />
		</v-main>

		<site-footer :copyright="copyright" />
	</v-app>
</template>

<script>
import HelloWorld from "./components/HelloWorld";
import SiteDrawer from "./components/SiteDrawer.vue";
import SiteFooter from "./components/SiteFooter.vue";
import SiteTitle from "./components/SiteTitle.vue";

export default {
	name: "App",
	components: {
		HelloWorld,
		SiteTitle,
		SiteFooter,
		SiteDrawer,
	},

	data: () => ({
		drawer: false,
		copyright: "ezcode",
		site: {
			title: "페이지 타이틀",
			menu: [
				{ title: "Home", to: "/" },
				{ title: "About", to: "/about" },
			],
		},
	}),
	methods: {
		save() {
			let userId = "ezcode";
			let name = "남기용";
			let email = "moosin76@gmail.com";
			this.$firebase.database().ref().child(userId).set({
				title: name,
				text: email,
			});
		},
		read() {
			let userId = "ezcode";
			this.$firebase
				.database()
				.ref()
				.child(userId)
				.on("value", (sn) => {
					console.log("on", sn.val());
				});
		},
		async readOnce() {
			let userId = "ezcode";
			const sn = await this.$firebase
				.database()
				.ref(userId)
				.once("value");
			console.log("once", sn.val());
		},
		subcribe() {
			this.$firebase
				.database()
				.ref("site")
				.on("value", (sn) => {
					const v = sn.val();
					
					if(!v) {
						this.$firebase.database().ref('site').set(this.site);
					}
					this.site = v;
					console.log(this.site);
				}, (e) => {
					console.log(e.message);
				});
		},
	},
	created() {
		this.subcribe();
	},
	mounted() {
		// console.log(this.$firebase);
	},
};
</script>
