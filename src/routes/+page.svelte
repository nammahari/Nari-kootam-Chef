<script>
	import { GoogleGenerativeAI } from '@google/generative-ai';
	let items = '';
	let count = '';
	let output = '';
	let loading = false;

	const genAI = new GoogleGenerativeAI('AIzaSyB-eG9Mzr3tMqVWuK4y2u3rs4aaXX3YqcE');
	async function getOutput() {
		loading = true;
		const model = genAI.getGenerativeModel({ model: 'gemini-pro' });
		const prompt = `Your an AI Chef who is going to help with cooking proper Indian Dishes. mostly south indian . lets put like this your goign to recommend easy cooking dishes. get the ingridents they have and number of people they want to cook and reccomdent them the most simple and easy food they can make out of that with ingredients with count/grams that should be enough for the number of people they cook, also the procedure to cook with duration. also mention the name of the dish with duration to cook.

your going to help the most bachelors who are going to cook in home. so recommend simple and easy and tasty food with the ingridents they have. also explain the procedures like your doing for a kid

return with step by step procedure !

Ingridents : ${items}
Number of People : ${count}

The Output should look like :

Name of the Dish - Duration to make for the count.

Ingridents :

list of the things - count/grams

Procedure :

things to do for duration or state`;

		try {
			const result = await model.generateContent(prompt);
			output = result.response.text();
		} catch (error) {
			console.error('Error:', error);
			output = 'Sorry, make some idiot pooped my site - try again after some time';
		} finally {
			loading = false;
		}
	}
</script>

<main>
	<h1>Nari kootam AI Chef</h1>
	<form on:submit|preventDefault={getOutput}>
		<label for="items">what you have ? </label>
		<input id="items" bind:value={items} required />

		<label for="count">how many people you got ?</label>
		<input id="count" bind:value={count} required />

		<button type="submit" disabled={loading}> Get the Recipe </button>
	</form>

	{#if loading}
		<p>wait karo...</p>
	{:else if output}
		<h2>Ta Da ! Try this Out:</h2>
		<p>{@html output.replace(/\n/g, '<br>')}</p>
	{/if}
</main>

<style>
	main {
		max-width: 600px;
		margin: 0 auto;
		padding: 20px;
	}
	form {
		display: flex;
		flex-direction: column;
		gap: 10px;
		margin-bottom: 20px;
	}
	input,
	button {
		padding: 5px;
	}
</style>
