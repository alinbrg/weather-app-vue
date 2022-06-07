<template>
	<div class="search-box">
		<input
			type="text"
			class="search-bar"
			placeholder="Search..."
			:value="q"
			@keypress="$emit('search', $event.target.value, $event)"
		/>
	</div>
</template>

<script>
export default {
	name: "SearchBoxComp",

	props: {
		q: String,
	},
};
</script>
