<script setup>
import { ref } from 'vue';

const conditions = ref([
	{ attr: '', ope: '', value: '', optional: false } // Condition initiale
]);

const addCondition = () => {
	conditions.value.push({ attr: '', ope: '', value: '', optional: false });
};

const removeCondition = index => {
	conditions.value.splice(index, 1);
};

const submitForm = () => {
	const result = {};

	conditions.value.forEach(condition => {
		const { attr, ope, value, optional } = condition;
		const conditionObj = {
			condition: {
				attr,
				ope,
				value
			}
		};

		if (optional) {
			if (!result.$at_least_one) {
				result.$at_least_one = [];
			}

			result.$at_least_one.push({ $all: [conditionObj] });
		} else {
			if (!result.$all) {
				result.$all = [];
			}

			result.$all.push(conditionObj);
		}
	});

	console.log(result);
	console.log(JSON.stringify(result));
};
</script>

<template>
	<div class="max-w-md mx-auto bg-white p-6 rounded-md shadow-md">
		<h2 class="text-lg font-semibold mb-4">Générer JSON</h2>
		<form id="filterForm" class="space-y-4" @submit.prevent="submitForm">
			<!-- Conditions début -->
			<div v-for="(condition, index) in conditions" :key="index">
				<div>
					<label class="block">Attribut</label>
					<input
						type="text"
						class="border border-gray-300 rounded-md px-4 py-2 w-full"
						v-model="condition.attr" />
				</div>

				<div>
					<label class="block">Opérateur</label>
					<input
						type="text"
						class="border border-gray-300 rounded-md px-4 py-2 w-full"
						v-model="condition.ope" />
				</div>

				<div>
					<label class="block">Valeur</label>
					<input
						type="text"
						class="border border-gray-300 rounded-md px-4 py-2 w-full"
						v-model="condition.value" />
				</div>

				<div>
					<label class="block">Optionnel</label>
					<input
						type="checkbox"
						class="border border-gray-300 rounded-md px-4 py-2 w-full"
						v-model="condition.optional" />
				</div>

				<!-- Supprimer la condition -->
				<button
					type="button"
					class="text-red-500"
					@click="removeCondition(index)">
					Supprimer
				</button>
			</div>
			<!-- Conditions fin -->

			<button
				type="button"
				class="bg-gray-200 text-gray-800 px-4 py-2 rounded-md hover:bg-gray-300 transition-colors"
				@click="addCondition">
				Ajouter une Condition
			</button>

			<button
				type="submit"
				class="bg-blue-500 text-white px-4 py-2 rounded-md hover:bg-blue-600 transition-colors">
				Filtrer
			</button>
		</form>
	</div>
</template>
