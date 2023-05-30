<script>

	import settings from "../etc/settings.json";

	const path = "http://localhost:8080/iframe";
	let amount = 0.01; // amount for the iframe to charge
	let text = "Click a button to generate a link";
	let url = "/";

	const encodeSettings = () => { // we can do this in one line, but seperating for clarity
		const jsonString = JSON.stringify(settings);
		const urlEncoded = encodeURIComponent(jsonString); // the compiler should optimize anyway
		return `${path}/${btoa(urlEncoded)}`; // build the URL with base64 encoded data
	};

	const generateActualURL = () => {
		settings.amount = amount;
		text = `Visit page to charge $${amount}`;
		url = encodeSettings();
	};

	const generateRandomURL = () => {
		const dollars = Math.round(Math.random() * 10000000);
		const cents = Math.round(Math.random() * 100);
		settings.amount = (dollars + (cents / 100)); // max $10,000.00
		text = `Visit page to charge $${randomAmount.toFixed()}`;
		url = encodeSettings();
	}

</script>

<div id="card">
	<a href={url}>{text}</a>
	<div>
		<span id="price-input-label">iframe will charge: $</span>
		<input id="price-input" type="number" bind:value={amount} step="0.01" />
	</div>
	<div id="buttons">
		<button on:click={generateActualURL}>Generate Test Payment Link</button>
		<button on:click={generateRandomURL}>I'm Feeling Lucky (Random Price)</button>
	</div>
</div>

<style>
	#price-input {
		margin: 20px 0;
		width: 50px;
	}
	#price-input-label {
		width: min-content;
	}
	#link-output {
		hyphens: auto;
		font-weight: bolder;
		width: 95%;
		height: 100px;
		text-align: center;
		color: orangered;
	}
	#card {
		font-family: Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif;
		width: 50%;
		margin: 15% auto;
		padding: 50px 5px;
		text-align: center;
		border-radius: 15px;
		box-shadow: 2px 2px 8px;
	}
	button {
		font-weight: bolder;
		margin: 0 20px;
		padding: 10px 20px;
		border-radius: 10px;
		background-color: transparent;
		color: darkorange;
		border: 1px solid orangered;
		cursor: pointer;
	}
	button:hover {
		background-color: orangered;
		color: white;
	}
</style>