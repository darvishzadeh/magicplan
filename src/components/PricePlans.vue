<template>
	<v-card max-width="900" class="mx-auto my-5 text-center" raised>

		<div class="pt-5 title font-weight-bold grey--text text--darken-4">Subscription</div>
		<div class="mb-5 grey--text text--darken-3">Subscribe to create as many Floor Plans as you need</div>

		<div class="subscriptionLengthToggle">
			<v-btn-toggle
				class="mt-2 inset-border-1"
				v-model="subscrpition_mode"
				mandatory
			>
				<v-btn text small color="blue" class="text-capitalize" height="32" width="100">Monthly</v-btn>
				<v-btn text small color="blue" class="text-capitalize" height="32" width="100">Yearly</v-btn>
			</v-btn-toggle>
		</div>

		<v-card-text class="px-0">
			<v-layout>
				<v-flex xs12>
					<v-toolbar
						color="blue lighten-2"
						dark
						flat
						dense
						class="py-1 text-center"
					>
						<v-flex xs6><div class="title font-weight-bold">Standard</div></v-flex>
						<v-divider
							vertical
						/>
						<v-flex xs6><div class="title font-weight-bold">Business</div></v-flex>
					</v-toolbar>
				</v-flex>
			</v-layout>

			<v-layout>
				<v-flex class="pt-5" xs6>
					<div class="display-1 grey--text text--darken-4">{{ prices.currency.symbol}}{{ StandardPricePerMonth }}</div>

					<div class="mt-1 mb-3 grey--text text--darken-4">user per month</div>

					<v-btn
						color="primary"
						dark
						depressed
						@click="subscribe('standard')"
					>Subscribe</v-btn>

					<v-divider class="ma-2" />

					<div v-if="subscription.standard">
						<div class="py-5">
							&nbsp;<span v-if="subscrpition_mode === 1">{{prices.currency.symbol}}{{prices.standard.yearly}} / Year</span>
						</div>

						<div class="px-2 py-5 grey lighten-4">
							<v-btn outlined block color="blue">PayPal</v-btn>
							or
							<v-btn outlined block color="blue">credit card</v-btn>
						</div>
					</div>

					<ul class="list grey--text text--darken-4 text-center">
						<li>Floor plan creation</li>
						<li>Backup in the magicplan Cloud</li>
						<li>Material &amp; Cost Estimator</li>
						<li>
							Export all plans as PDF, JPG, DXF, PNG, SVG and CSV
							files
						</li>
						<li>Access all objects</li>
						<li>3D Renderings</li>
						<li>Virtual Tours</li>
						<li>-</li>
						<li>-</li>
						<li>-</li>
						<li>-</li>
					</ul>
				</v-flex>

				<v-divider
					vertical
				/>

				<v-flex class="pt-5" xs6>
					<div class="display-1 grey--text text--darken-4">{{ prices.currency.symbol}}{{ BusinessPricePerMonth }}</div>

					<div class="mt-1 mb-3 grey--text text--darken-4">device per month</div>

					<v-btn
						color="primary"
						dark
						depressed
						@click="subscribe('business')"
					>Subscribe</v-btn>

					<v-divider class="ma-2" />

					<div v-if="subscription.business">
						<div class="py-5">
							<v-layout>
								<v-flex>Devices:</v-flex>
								<v-flex>
									<v-btn :disabled="numberMultiplier <= 1" @click="changeMultiplier(-1)" small>-</v-btn>
									{{numberMultiplier}}
									<v-btn @click="changeMultiplier(1)" small>+</v-btn>
								</v-flex>
								<v-flex>{{prices.currency.symbol}}{{calculatedPrice}}</v-flex>
							</v-layout>
						</div>

						<div class="px-2 py-5 grey lighten-4">
							<v-btn outlined block color="blue">PayPal</v-btn>
							or
							<v-btn outlined block color="blue">credit card</v-btn>
						</div>
					</div>

					<ul class="list grey--text text--darken-4 text-center">
						<li>Floor plan creation</li>
						<li>Backup in the magicplan Cloud</li>
						<li>Material &amp; Cost Estimator</li>
						<li>
							Export all plans as PDF, JPG, DXF, PNG, SVG and CSV
							files
						</li>
						<li>Access all objects</li>
						<li>3D Renderings</li>
						<li>Virtual Tours</li>
						<li class="font-weight-bold">Multi-License Management</li>
						<li class="font-weight-bold">Create Objects</li>
						<li class="font-weight-bold">Create Surveys</li>
						<li class="font-weight-bold">Create Pricelists</li>
					</ul>
				</v-flex>
			</v-layout>
		</v-card-text>
	</v-card>
</template>

<script>
import prices from "../config/prices.js";

export default {
	data: () => ({
		subscription: {
			standard: false,
			business: false
		},
		prices: prices,
		subscrpition_mode: 1,
		numberMultiplier: 1
	}),
	created: () => {},
	computed: {
		StandardPricePerMonth() { return this.subscrpition_mode === 0 ? prices.standard.monthly : (prices.standard.yearly / 12).toFixed(2) },
		BusinessPricePerMonth() { return this.subscrpition_mode === 0 ? prices.business.monthly : (prices.business.yearly / 12).toFixed(2) },
		BusinessPrice() { return this.subscrpition_mode === 0 ? prices.business.monthly : prices.business.yearly },
		calculatedPrice() { return (this.numberMultiplier * this.BusinessPrice).toFixed(2) }
	},
	methods: {
		subscribe(subscrpition_name) {
			this.subscription[subscrpition_name] = !this.subscription[subscrpition_name];
		},
		changeMultiplier(amount) {
			this.numberMultiplier = this.numberMultiplier + amount;
		},
	}
};
</script>

<style lang="scss" scoped>
.subscriptionLengthToggle {
	.inset-border-1 {
		box-shadow: inset 0 0 0px 1px #1580dd !important;
	}

	.v-btn--active {
		background-color: #1976d2;
		color: white !important;
	}
}

.list {
	list-style-type: none;
}

.list > li {
	padding: 5px;
}
</style>
