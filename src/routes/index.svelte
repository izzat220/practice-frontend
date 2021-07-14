<script>
	import axios from "axios";
	import { onMount } from "svelte";

	let users;

	onMount(async () => {
		let response = await axios.get("http://localhost:8080/getStuff");
		users = response.data.results;
		console.log(users);
	});
</script>

{#if users}
	<table class="table">
		<thead>
			<tr>
				<th scope="col">FirstName</th>
				<th scope="col">LastName</th>
				<th scope="col">Age</th>
			</tr>
		</thead>
		<tbody>
			{#each users as user, i}
				<tr>
					<td>{user.firstName}</td>
					<td>{user.lastName}</td>
					<td>{user.age}</td>
				</tr>
			{/each}
		</tbody>
	</table>
{:else}
	<div class="spinner-border" role="status">
		<span class="visually-hidden">Loading...</span>
	</div>
{/if}
