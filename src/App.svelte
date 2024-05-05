<script>
	import { Notyf } from "../node_modules/notyf";
	const events = [
		[
			"App Development",
			"Web Development",
			"Game Development",
			"Encryptid | Cryptic Hunt X CTF",
		],
		["3D Design", "2D Design", "UI Design", "A/V editing"],
		["Group Discussion", "Crossword", "Gaming", "Photography"],
		["Competitive Programming", "Film Making", "Hardware", "Cubing 2 by 2"],
		["Cubing 3 by 3"],
	];

	let selected = [];
	let notyf = new Notyf();

	function removeSpace() {
		var email = window.document.getElementById("email").value;
		email = email.replace(/\s/g, "");
		window.document.getElementById("email").value = email;
	}

	function submit() {
		console.log("hey");
		var vehicle1 = document.getElementById("vehicle1").checked;
		var name = window.document.getElementById("name").value;
		var dob = window.document.getElementById("dob").value;
		var email = window.document.getElementById("email").value.trim();
		var phone = window.document.getElementById("phone").value;
		var adno = window.document.getElementById("adno").value;
		var classs = window.document.getElementById("class").value;
		var section = window.document.getElementById("section").value;
		if (!vehicle1) {
			notyf.error("Please ask your parents to consent for Discord!");
			return;
		}
		if (!(name && dob && email && phone && adno && classs && section)) {
			notyf.error("Please fill all the fields");
			return;
		}
		if (
			new Date(dob).getFullYear() < 2000 ||
			new Date(dob).getFullYear() > 2015
		) {
			notyf.error("Please enter a valid date of birth");
			return;
		}
		if (phone.length != 10) {
			notyf.error("Please enter a valid phone number");
			return;
		}
		// email regex
		var re =
			/^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
		if (!re.test(email)) {
			notyf.error("Please enter a valid email");
			return;
		}
		if (adno.length < 4 || adno.length > 5) {
			notyf.error("Please enter a valid admission number");
			return;
		}
		if (classs < 1 || classs > 12) {
			notyf.error("Please enter a valid class");
			return;
		}
		window.document.getElementById("reg-container").style.display = "none";
		window.document.getElementById("reg-container-2").style.display =
			"flex";
	}

	function select(string) {
		var x = document.getElementById(string);
		if (x.style.color == "rgb(255, 255, 255)") {
			x.style.border = "3px solid #16e16e";
			x.style.color = "#16e16e";
			selected.push(string);
		} else {
			x.style.color = "rgb(255, 255, 255)";
			x.style.border = "3px solid #2C2A2A";
			var index = selected.indexOf(string);
			if (index > -1) {
				selected.splice(index, 1);
			}
		}
		console.log(selected);
	}

	async function register() {
		var name = window.document.getElementById("name").value;
		var dob = window.document.getElementById("dob").value;
		var email = window.document.getElementById("email").value;
		var phone = window.document.getElementById("phone").value;
		var adno = window.document.getElementById("adno").value;
		var classs = window.document.getElementById("class").value;
		var section = window.document.getElementById("section").value;
		if (selected.length == 0) {
			notyf.error("Please select atleast one field");
			return;
		}
		var body = JSON.stringify({
			name: name,
			dob: dob,
			email: email,
			phone: phone,
			adno: adno,
			grade: classs,
			section: section,
			selected: selected,
		});
		window.document.getElementById("regis").disabled = true;
		await fetch("https://intech-reg-24.onrender.com/register", {
			method: "POST",
			headers: {
				"Content-Type": "application/json",
			},
			body: body,
		}).then(async (response) => {
			response = await response.json();
			console.log(response);
			if (
				response.hasOwnProperty("success") &&
				response.success == false
			) {
				notyf.error(response.msg);
				window.document.getElementById("regis").disabled = false;
				return;
			}
			window.document.getElementById("reg-container-2").style.display =
				"none";
			window.document.getElementById("regMail").innerText = email;
			window.document.getElementById("thank-you").style.display = "flex";
		});
	}
</script>

<main>
	<script src="https://cdn.jsdelivr.net/npm/notyf@3/notyf.min.js"></script>
	<div class="nav">
		<img
			src="https://github.com/techsyndicate/website/blob/main/public/assets/images/ts.png?raw=true"
		/>
	</div>
	<div id="reg-container" class="reg-container">
		<h1 class="title">Register</h1>
		<div class="reg-form">
			<div class="reg-form-div-container">
				<div class="reg-form-div">
					<p for="name">Name</p>
					<input
						id="name"
						type="text"
						name="name"
						placeholder="John Doe"
					/>
				</div>
				<div class="reg-form-div">
					<p for="dob">Date Of Birth</p>
					<input id="dob" type="date" name="dob" />
				</div>
			</div>
			<div class="reg-form-div-container">
				<div class="reg-form-div">
					<p for="email">School Email</p>
					<input
						type="text"
						id="email"
						name="email"
						on:input={removeSpace}
						placeholder="john.doe@ais.amity.edu"
					/>
				</div>
				<div class="reg-form-div">
					<p for="phone">Phone Number</p>
					<input
						type="number"
						id="phone"
						name="phone"
						placeholder="9889889889"
					/>
				</div>
			</div>
			<div class="reg-form-div-container">
				<div class="reg-form-div">
					<p for="adno">Admission Number</p>
					<input
						type="number"
						id="adno"
						name="adno"
						placeholder="1111"
					/>
				</div>
				<div class="reg-form-div">
					<p for="class">Grade/Class</p>
					<input
						type="number"
						id="class"
						name="class"
						placeholder="9"
					/>
				</div>
			</div>
			<div class="reg-form-div-container">
				<div class="reg-form-div">
					<p for="section">Section</p>
					<input
						type="text"
						id="section"
						name="section"
						placeholder="A"
					/>
				</div>
			</div>
			<div class="reg-form-div-container-parent">
				<input
					type="checkbox"
					id="vehicle1"
					name="vehicle1"
					value="Bike"
				/>
				<label
					style="color: #16e16e; font-size: 1.3rem;"
					for="vehicle1"
				>
					I consent for my child if above 13 to join Discord Server of
					inTech (filled by parent).
				</label><br />
			</div>
		</div>
		<div class="inpt-sub-div-1">
			<button class="inpt-sub" on:click={submit}>Continue</button>
		</div>
	</div>
	<div id="reg-container-2" class="reg-container">
		<h1 class="title">Choose Interested Fields</h1>
		<div class="reg-form-2">
			{#each events as event}
				<div class="reg-form-div-container">
					{#each event as eventEl}
						<button
							style="color: rgb(255, 255, 255); border: #2C2A2A solid 3px;"
							class="event-but"
							on:click={() => select(eventEl)}
							id={eventEl}>{eventEl}</button
						>
					{/each}
				</div>
			{/each}
		</div>
		<div class="inpt-sub-div">
			<button id="regis" class="inpt-sub" on:click={register}
				>Submit</button
			>
		</div>
	</div>
	<div id="thank-you" class="reg-container">
		<h1 class="title">Thank You</h1>
		<div class="reg-form-2">
			<p class="thank-you-p">
				Your registration is complete. You have been emailed the details
				at <span id="regMail"></span>
				to proceed further. <br />
				(Make sure to check spam mail)
			</p>
		</div>
	</div>
</main>

<style>
	@import url("https://fonts.googleapis.com/css2?family=Outfit:wght@100;200;300;400;500;600;700;800;900&display=swap");
	:global(body) {
		margin: 0;
		padding: 0;
	}
	.reg-form-div-container-parent {
		display: flex;
		justify-content: center;
		align-items: center;
		width: 100%;
	}

	.reg-form-div-container-parent input {
		width: 20px !important;
		color: #16e16e;
	}

	.event-but {
		background-color: black;
		padding: 15px 32px;
		text-align: center;
		text-decoration: none;
		display: inline-block;
		font-size: 16px;
		margin: 1vw;
		cursor: pointer;
		border-radius: 10px;
		width: 20vw;
		font-family: "Outfit", sans-serif;
	}

	#reg-container {
		margin-bottom: 50px;
	}

	#reg-container-2 {
		display: none;
	}

	#thank-you {
		display: none;
	}
	#regMail {
		text-decoration: underline;
	}

	.thank-you-p {
		font-size: 1.5rem;
		font-family: "Outfit", sans-serif;
		color: #fff;
	}

	.nav img {
		width: 70px;
		margin: 10px;
	}

	.title {
		color: #16e16e;
		width: 80vw;
		font-size: 4rem;
		font-weight: 600;
		font-family: "Outfit", sans-serif;
	}

	:global(body) {
		background-color: #000;
		margin: 0;
		font-family: "Outfit", sans-serif;
	}

	.inpt-sub-div-1 {
		width: 60vw;
		display: flex;
		justify-content: flex-end;
	}

	.inpt-sub-div {
		width: 80vw;
		display: flex;
		justify-content: flex-end;
	}

	.inpt-sub {
		margin-top: 15px;
		width: 18vw !important;
		padding-top: 0.8vw;
		padding-bottom: 0.8vw;
		background-color: #16e16e;
		font-size: 1.5rem;
		border-radius: 9px;
		font-family: "Outfit", sans-serif;
		cursor: pointer;
		font-weight: 400;
	}

	.reg-form {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		margin-bottom: 0;
	}

	.reg-form p {
		width: 40vw;
		color: #16e16e;
		font-size: 1.5rem;
		font-family: "Outfit", sans-serif;
	}

	.reg-form-div {
		display: flex;
		flex-direction: column;
		margin-bottom: 1vw;
		width: 40vw;
	}

	.reg-form-div-container {
		display: flex;
		width: 80vw;
		justify-content: space-between;
		font-family: "Outfit", sans-serif;
	}

	.reg-form input {
		height: 40px;
		width: 30vw;
		border-radius: 7px;
		padding: 1.3vw;
		background-color: #000;
		border: #2c2a2a solid 2px;
		font-size: 1rem;
		color: #fff;
		font-family: "Outfit", sans-serif;
	}

	input[type="date"]::-webkit-calendar-picker-indicator {
		filter: invert(1);
	}

	.reg-container {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	@media screen and (max-width: 1020px) {
		.event-but {
			width: 80vw;
			font-size: 3rem;
		}

		.title {
			font-size: 8rem;
		}

		.nav img {
			width: 100px;
		}

		.reg-form p {
			font-size: 3rem;
			margin: 0;
			margin-bottom: 5px;
			width: 90vw;
		}

		.reg-form-div-container {
			flex-direction: column;
		}

		.reg-form input {
			width: 80vw;
			height: 120px;
			font-size: 3rem;
		}

		.inpt-sub {
			width: 80vw !important;
			height: 100px;
			font-size: 3rem;
		}

		.inpt-sub-div,
		.inpt-sub-div-1 {
			width: 80vw;
			margin-bottom: 50px;
		}

		.reg-form-div {
			margin-bottom: 5vw;
		}

		.thank-you-p {
			font-size: 3rem;
			text-align: center;
		}

		.reg-form-div-container-parent input {
			width: 100px !important;
		}

		.reg-form-div-container-parent label {
			font-size: 2.3rem !important;
			margin-left: 35px;
		}

		.reg-form-div-container-parent {
			width: 80vw;
			margin-bottom: 50px;
		}
	}
</style>
