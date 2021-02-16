<template>
	<v-footer dark fixed>
		<v-spacer />
		<div>&copy; {{year}} {{copyright}}</div>
	</v-footer>
</template>

<script>
export default {
    props :["copyright"],
    computed : {
        year() {
            return new Date().getFullYear()
        },
    }
};
</script>

<style>
</style>