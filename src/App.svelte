<script>
	import { onMount, onDestroy } from "svelte";

	export let name;
	let schedulers = [];
	let binance_assets = null;
	let pancake_lp = null;
	let all_assets = null;

	async function get_binance_data () {
		var res = await fetch("api/binance_assets");
    	binance_assets = await res.json();
		binance_assets = binance_assets["res"]
	}

	onMount(async () => {
		get_binance_data();
		schedulers.append(setInterval(() => {get_binance_data()}, 5000));
		
	});

	onDestroy(() => schedulers.forEach(element => {
		clearInterval(element);
	}));

	

</script>
	<nav class="navbar navbar-expand-md navbar-dark bg-dark">
		<a class="navbar-brand" href="#">Crypto App</a>
        <ul class="navbar-nav w-100">
            <li class="nav-item"><a href="#" class="nav-link">Home</a></li>
            <li class="nav-item"><a href="#" class="nav-link">Link</a></li>
            <!--<li class="nav-item ml-md-auto"><a href="#" class="btn btn-primary switch">Switch Light/Dark</a></li>-->
        </ul>
	</nav>
	<section class="container">
	<div class="row">
		<div class="col-md-12">
			<h2>All Assets 
				{#if all_assets != null}
				(${all_assets.total_usd.toFixed(2)})
				{/if}

			</h2>
			<table class="table table-dark">
				<th>Symbol</th>
				<th>Amount</th>
				<th>USD Value of Token</th>
				<th>USD Value Held</th>
					{#if all_assets != null}
						{#each Object.entries(all_assets.token_stats) as [token_name, token_stats]}
						<tr>
							<td>{token_name}</td>
							<td>{token_stats.count}</td>
							<td>${token_stats.current_price}</td>
							<td>${(token_stats.count * token_stats.current_price).toFixed(2)}</td>
						</tr>
						{/each}
					{:else}
						<tr>
							<td>Loading...</td>
						</tr>
					{/if}
			</table>
		</div>
	</div>
	<div class="row">
		<div class="col-md-4">
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

