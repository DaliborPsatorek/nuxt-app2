<script setup>
	defineProps(["person"])
	const date = new Date()
	let year = date.getFullYear()
	let personAge = (birthday) => {
		return ref(year - birthday), year - birthday
	}
</script>

<template>
	<section v-bind:key="person" v-bind:class="{ 'card-container': true, card: true, selected: false ,adult: personAge(person.birthday) >= 18 }">
		<article class="info">
			<div class="name">
				{{ person.fname }} {{ person.lname }}
				<div class="birthday"> <span class="year-title">year</span> {{ person.birthday }}</div>
				<div class="trueAge">{{ personAge(person.birthday) }} <span class="age">years old</span></div>
			</div>
		</article>
		<footer class="footer-card">
			<span @click="$emit('editWasClicked', person)" class="btn edit">Edit</span>
			<span @click="$emit('removeWasClicked', person)" class="btn remove">Remove</span>
		</footer>
	</section>
</template>

<style scoped>
	.card {
		background: rgba(255, 255, 255, 1);
		border: 1px solid #bbb;
		color: #444;
		box-shadow: 0 3px 5px rgba(0, 0, 0, 0.2);
		border-top-width: 5px;
		border-top-color: dodgerblue;
	}
	.card.adult {
		border-top-color: orange;
	}
	.card:hover {
		box-shadow: 10px 5px 5px gray;
	}
	.info {
		padding: 1rem;
		text-align: center;
		font-size: 1.3rem;
	}
	.name {
		font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
	}
	.birthday {
		font-weight: 300;
		font-size: 1rem;
	}
	.year-title {
		color: #bbbbbb;
	}
	.footer-card {
		display: flex;
	}
	.btn {
		padding: 0.75rem 2rem;
		color: white;
		width: 50px;
		flex-grow: 1;
		text-align: center;
		transition: 0.5s;
		font-size: 1rem;
	}
	.btn.edit {
		background: rgb(91, 236, 91);
	}
	.btn.remove {
		background: red;
	}
	.btn:hover {
		opacity: 0.3;
		cursor: pointer;
	}
</style>