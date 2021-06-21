<script>
	import { onMount } from "svelte";
	let amount = 1;
	$: sum = amount * 2;
	onMount(() => {
		paypal
			.Buttons({
				createOrder: (data, actions) => {
					return actions.order.create({
						purchase_units: [
							{
								amount: {
									value: sum,
								},
							},
						],
					});
				},
				onApprove: (data, actions) => {
					return actions.order.capture().then((details) => {
						console.log("Transaction done by " + details.payer.name.given_name);
						console.log(details.payer);
					});
				},
			})
			.render("#paypal-button-container");
	});
</script>

<style>
	h1 {
		font-size: 3rem;
	}
</style>

<h1>Welcome to my Store! Your Bill is: ${sum}</h1>
<input type="text" bind:value={amount} />
<div id="paypal-button-container" class="relative z-10" />
