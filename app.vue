<template>
	<main>
		<h1>Zoznam ľudí</h1>
		<section class="list">
			<section
				v-for="person of persons"
				v-bind:key="person"
				v-bind:class="{
					'card-container': true,
					card: true,
					adult: personAge(person.birthday) >= 18,
				}"
			>
				<PersonalCard v-bind:person="person" @removeWasClicked="removePerson" @editWasClicked="editPersonSelection"></PersonalCard>
			</section>
		</section>
		<div v-show="editMode" class="inputs">
			<input v-model="InName" placeholder="First Name" />
			<input v-model="InSurname" placeholder="Last Name" />
			<input v-model="InBirthyear" placeholder="Year of birth" />
		</div>
		<div class="buttonHolder">
			<div v-show="state" class="button" id="delete-button" @click="deleteAll">DELETE ALL</div>
			<div v-show="state == false" class="button" id="add-button" @click="addAll">ADD ALL</div>
			<div v-show="InName != false || InSurname != false || InBirthyear !=''   " class="button" id="clear-input-button" @click="clearInput">Clear Imput</div>
			<div v-show="InName != false && InSurname != false && InBirthyear !=''   " class="button" id="send-endit" @click="setEdit"  >Set Edit</div>
			<!--exit-->
			<div v-show="editMode == true" @click="exitEditMode" class="button" id="exit-edit-mode">Exit Edit</div>
		</div>
	</main>
</template>

<script setup>
	const date = new Date()
	let year = date.getFullYear()
	const allpersons = [
		{
			fname: "Magdalena",
			lname: "Mikulova",
			birthday: 2000,
		},
		{
			fname: "Mirka",
			lname: "Makovicova",
			birthday: 1992,
		},
		{
			fname: "Hugo",
			lname: "Hugovic",
			birthday: 2007,
		},
		{
			fname: "Miki",
			lname: "Hric",
			birthday: 2003,
		},
		{
			fname: "Andrea",
			lname: "Veresova",
			birthday: 1982,
		},
		{
			fname: "Nora",
			lname: "Mojsejova",
			birthday: 2009,
		},
	]
	let oldpersons = allpersons
	let persons = ref(allpersons)
	let personAge = (birthday) => {
		return ref(year - birthday), year - birthday
	}
	let state = ref(true)
	let deleteAll = () => {
		persons.value = []
		state = false
		InName.value = ""
		InSurname.value = ""
		InBirthyear.value = ""
	}
	let addAll = () => {
		persons.value = allpersons
		state = true
	}
	const removePerson = (personShouldBeRemoved) => {
		if(editMode != true){
			persons.value = persons.value.filter((item) => {
				return item != personShouldBeRemoved
			})
			oldpersons = persons.value
		}
	}
	let InName = ref("");
	let InSurname = ref("");
	let InBirthyear = ref("");
	let editMode = false;
	let editPersonSelection = (personShouldBeAdded) => {
		persons.value = persons.value.filter((item) => {
			editMode = true
			return item == personShouldBeAdded
		})
	}

	const setEdit = () => {
		let newPerson = persons;
		newPerson.value[0].fname = InName.value;
		newPerson.value[0].lname = InSurname.value;
		newPerson.value[0].birthday = InBirthyear.value
		persons.value = oldpersons
		editMode = false
		clearInput()
	}
	const exitEditMode = () => {
		persons.value = oldpersons
		editMode = false
		clearInput()
	}
	const clearInput = () => {
		InName.value = ""
		InSurname.value = ""
		InBirthyear.value = ""
	}

</script>

<style>
	@import url("https://fonts.googleapis.com/css2?family=Oswald:wght@200;300;500;600;700&display=swap");
	body {
		background-color: #9fb2bc70;
		font-size: 10px;
		font-family: "Oswald", sans-serif;
		padding: 1rem;
	}
	main {
		display: flex;
		flex-direction: column;
		gap: 0.75rem;
	}
	.list {
		display: flex;
		gap: 20px;
		flex-direction: row;
		flex-wrap: wrap;
		justify-content: stretch;
		font-size: 1.2rem;
	}
	.card-container {
		flex-grow: 1;
	}
	.button {
		text-align: center;
		font-size: xx-large;
		height: 3rem;
		width: 20rem;
		color: white;
		background-color: red;
		margin: 1rem;
	}
	.button:hover {
		box-shadow: 10px 5px 5px gray;
	}
	.buttonHolder {
		display: flex;
	}
	#clear-input-button {
		background-color: gray;
	}
	#send-endit{
		background-color: green;
	}
	#exit-edit-mode{
		background-color: greenyellow
	}
</style>