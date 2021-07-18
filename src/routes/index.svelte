<script>
	import axios from "axios";
	import { onMount } from "svelte";

	let users;

	let rowsToEdit = [];

	const getUsers = async () => {
		let response = await axios.get("http://localhost:8080/getUsers");
		users = response.data;
		console.log(users);
	};

	const editRow = (i) => {
		if (rowsToEdit.includes(i)) {
			let index = rowsToEdit.indexOf(i);
			rowsToEdit.splice(index, 1);
		} else {
			rowsToEdit.push(i);
		}

		rowsToEdit = rowsToEdit;
	};

	onMount(() => {
		getUsers();
	});

	const save = async (newFirstName, userId) => {
		console.log(users);
		let response = await axios.post("http://localhost:8080/updateUser", {
			userId: userId,
			newFirstName: newFirstName,
		});

		console.log(response);
		getUsers();
	};
</script>

<table class="table">
	<thead>
		<tr>
			<th scope="col">FirstName</th>
			<th />
			<th />
		</tr>
	</thead>
	<tbody>
		{#if users}
			{#each users as user, i}
				<tr>
					<td>
						{#if rowsToEdit.includes(i)}
							<input
								class="form-control form-control-sm"
								bind:value={user.firstName}
							/>
						{:else}
							{user.firstName}
						{/if}
					</td>
					<td
						><button class="btn btn-light btn-sm" on:click={() => editRow(i)}
							>Edit</button
						></td
					>
					<td
						><button
							class="btn btn-primary btn-sm"
							on:click={() => save(user.firstName, user._id)}>Save</button
						></td
					>
				</tr>
			{/each}
		{/if}
	</tbody>
</table>
