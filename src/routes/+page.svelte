<script>

	import settings from "../etc/settings.json";

	const path = "http://localhost:8080/iframe";
	let amount = 0.01; // amount for the iframe to charge
	let text = "";
	let url = "/";

	const setAmount = () => settings.amount = amount;
	const setText = () => text = `Click here to charge $${amount.toFixed(2)}`;

	const encodeSettings = () => { // we can do this in one line, but seperating for clarity
		const jsonString = JSON.stringify(settings);
		const urlEncoded = encodeURIComponent(jsonString); // the compiler should optimize anyway
		return `${path}/?params=${btoa(urlEncoded)}`; // build the URL with base64 encoded data
	};

	const generateActualURL = () => {
		setAmount();
		setText();
		url = encodeSettings();
	};

	const generateRandomURL = () => {
		amount = parseFloat((Math.random() * 1000).toFixed(2));
		generateActualURL();
	}

</script>

<div id="card">
	<a id="link-output" href={url} target="_blank">{text}</a>
	<div>
		<span>iframe will charge: $</span>
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
		width: 100px;
	}
	#link-output {
		hyphens: auto;
		text-decoration: none;
		font-weight: bolder;
		width: 95%;
		height: 100px;
		text-align: center;
		color: orangered;
		transition: 0.2s;
	}
	#link-output:hover {
		color: orange;
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
		transition: 0.2s;
	}
	button:hover {
		background-color: orangered;
		color: white;
	}
</style>