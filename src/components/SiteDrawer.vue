<template>
	<div>
		<v-list-item v-for="item in items" :key="item.title" link :to="item.to" >
			<v-list-item-content>
				<v-list-item-title>{{ item.title }}</v-list-item-title>
			</v-list-item-content>
		</v-list-item>
	</div>
</template>

<script>
export default {
	props: ["items"],
};
</script>

<style>
</style>