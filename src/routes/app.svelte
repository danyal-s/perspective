<script>
	import { onMount, onDestroy } from "svelte";

	export let name;
	let schedulers = [];
	let binance_assets = null;
	let page_selected = "home";

	async function get_binance_data () {
		var res = await fetch("api/binance_assets");
    	binance_assets = await res.json();
		binance_assets = binance_assets["res"]
	}

	onMount(async () => {
		get_binance_data();
		schedulers.push(setInterval(() => {get_binance_data()}, 5000));
		
	});

	onDestroy(() => schedulers.forEach(element => {
		clearInterval(element);
	}));

	function change_page(page_name) {
		page_selected = page_name
	}

</script>
	<nav class="navbar navbar-expand-md navbar-dark bg-dark">
		<a class="navbar-brand" href="#">Crypto App</a>
        <ul class="navbar-nav w-100">
            <li class="nav-item"><button id="haha" href="#" on:click={change_page} class="nav-link">Home</button></li>
            <li class="nav-item"><button href="#" on:click={change_page} class="nav-link">Link</button></li>
			<li class="nav-item"><a href="javascript:void(0);" on:click={() => {page_selected = "hi"}} class="nav-link">Link</a></li>
			<li class="nav-item"><button on:click={change_page} class="nav-link">Link</button></li>
			<li class="nav-item"><button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation"></button></li>
            <!--<li class="nav-item ml-md-auto"><a href="#" class="btn btn-primary switch">Switch Light/Dark</a></li>-->
        </ul>
	</nav>
	<section class="container">
		<p>{page_selected}</p>
	<div class="row">
		<div class="col-md-12">
			<h2>Binance Info</h2>
			<table class="table table-dark">
				<th>Symbol</th>
				<th>Amount</th>
				<th>USD Value of Token</th>
				<th>USD Value Held</th>
					{#if binance_assets != null}
						{#each binance_assets as k}
						<tr>
							<td>{k.asset}</td>
							<td>{k.free}</td>
							<td>${k.current_price.toFixed(2)}</td>
							<td>${k.amount_in_usd.toFixed(2)}</td>
						</tr>
						{/each}
					{:else}
						<tr>
							<td>Loading...</td>
						</tr>
					{/if}
			</table>
		</div>
		<div class="col-md-3">

		</div>
	<p></p>
</section>

