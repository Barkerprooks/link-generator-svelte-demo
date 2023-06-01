<script lang="ts">

	import settings from "../etc/settings.json";

	const path: string = "https://localhost:8080/iframe";

	let copied: boolean = false;
	let amount: number = 0.01; // amount for the iframe to charge
	let url: string = path;

	// Process the settings into a usable URL
	const encodeSettings = () => { // we can do this in one line, but seperating for clarity
		const jsonString = JSON.stringify(settings);
		const urlEncoded = encodeURIComponent(jsonString); // the compiler should optimize anyway
		return `${path}/?params=${btoa(urlEncoded)}`; // build the URL with base64 encoded data
	};

	// URL FUNCTIONS
	// simple utility to character wrap the URL so its not bleeding off the page or scrollable
	const wrapURL = (url: string, width: number) => {
		let wrapped: string = "";
		
		for (let i = 0; i < url.length; i++) // insert a new line for every position divisable by "width"
			wrapped += url[i] + (((i + 1) % width === 0) ? "\n" : "");

		return wrapped;
	};	
	// auto CTRL+C
	const copyUrl = () => { 
		navigator.clipboard.writeText(url);
		copied = true;
	};

	// BUTTON FUNCTIONS
	// mapped button function for the input value
	const generateActualURL = () => {
		settings.amount = amount;
		url = encodeSettings();
		copied = false;
	};
	// mapped button function for the random value
	const generateRandomURL = () => {
		amount = parseFloat((Math.random() * 1000).toFixed(2));
		generateActualURL();
	};

</script>

<div id="card">
	<div>
		<a id="link-output" href={url} target="_blank" style={`color: ${copied ? "orangered" : "black"}`}>{wrapURL(url, 64)}</a>
		{#if url !== path}
			<button on:click={copyUrl}>{copied ? "Copied!" : "Copy Link"}</button>			
		{/if}
	</div>
	<div>
		<span>Charge: $</span>
		<input id="price-input" type="number" bind:value={amount} step="0.01" />
	</div>
	<div id="buttons">
		<button on:click={generateActualURL}>Generate Test Payment Link</button>
		<button on:click={generateRandomURL}>I'm Feeling Lucky (Random Price)</button>
	</div>
</div>

<style>
	:global(html) {
		height: 100vh;
		background: linear-gradient(white, 80%,grey);
	}
	#price-input {
		margin: 50px 0;
		width: 100px;
	}
	#link-output {
		display: block;
		font-family: monospace;
		white-space: pre-wrap;
		margin: 20px 0;
		text-align: center;
		text-decoration: none;
		transition: 0.2s;
	}
	#link-output:hover {
		text-decoration: underline;
	}
	#card {
		font-family: Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif;
		background-color: white;
		width: 50%;
		margin: 10% auto;
		padding: 50px 5px;
		text-align: center;
		border-radius: 15px;
		box-shadow: 2px 3px 6px;
		transition: 0.2s;
	}
	button {
		display: inline;
		text-decoration: none;
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
