
<template>
	<html lang="en">
	<div class="squares">
		<div class="squareBlue">
			<p style="font-size:50px; color:aliceblue">{{ blueEnv }}</p>
		</div>
		<div class="squareGreen">
			<p style="font-size:50px; color:aliceblue">{{ greenEnv }}</p>
		</div>
	</div>

	</html>
</template>

 
<script>
import { ref, onMounted } from "vue";

export default {
	name: "HelloWorld",
	props: {},
	setup() {

		const response = ref("");
		const blueEnv = ref(0);
		const greenEnv = ref(0); 
		
		function fetchData() {
			return fetch(
				"https://signaloceantestapims.azure-api.net/devoxx/ServiceBusCount",{cache: 'no-cache'}
			)
				.then(async (res) => { 
					var textValue = await res.json();
					if (textValue.environment == "blue") {
						if (blueEnv.value<textValue.queueMessagesProccessed) {
							blueEnv.value = textValue.queueMessagesProccessed;
						}
					}
					if (textValue.environment == "green") {
						if (greenEnv.value<textValue.queueMessagesProccessed) {
							greenEnv.value = textValue.queueMessagesProccessed;
						} 
					}
				})
				.catch((err) => {
					throw err;
				});
		}

		onMounted(() => {
			setInterval(async () =>await fetchData(), 2000);
		});

		return {
			blueEnv,
			greenEnv,
			response, 
		};
	},
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.squares {
	display: flex;
	justify-content: center;
}

.squareGreen {
	text-align: center;
	display: inline-block;
	height: 150px;
	width: 150px;
	background-color: #008000;

	border-style: ridge;
	display: flex;
	align-items: center;
	justify-content: center;
}

.squareBlue {
	text-align: center;
	display: inline-block;
	height: 150px;
	width: 150px;
	background-color: #0000FF;
	margin-right: 5%;
	display: flex;
	border-style: ridge;
	display: flex;
	align-items: center;
	justify-content: center;

}
</style>
