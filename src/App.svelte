<script>
	import { onMount } from 'svelte';

	let books = [];

	async function handleResponse(response, successCallback) {
		if (response.status === 200) {
			await response.json().then(data => {
				console.log('Request succeeded with JSON response', data)
				successCallback(data)
			})
		} else {
			// todo
		}
	}

	async function getBooks(successCallback) {
		let url = 'https://edx7q5cw27.execute-api.eu-west-1.amazonaws.com/prod/bookstore/search'

		let response = await fetch(url, {
			method: 'POST',
			headers: {
				'Content-Type': 'application/json'
			},
			body: '"test"'
		})

		await handleResponse(response, successCallback)
	}

	onMount(() => {
		/*getBooks(function (body) {
			books = body
		})*/
	})

	function onSearchClick() {
		getBooks(function (body) {
			books = body
		})
	}

</script>

<h1>Bookstore</h1>

<button type="button" on:click={onSearchClick}>Search</button>

<table style="width: 100%">
	<tr>
		<th>Title</th><th>Author</th><th>Price</th><th>Info</th>
	</tr>
	{#each books as book}
	<tr>
		<td>{book.title} </td><td>{book.author}</td><td>{book.price}</td>
		<td>{#if book.isbn !== undefined}ISBN: {book.isbn}{/if} {#if book.publisher !== undefined}Publisher: {book.publisher}{/if}</td>
	</tr>
	{/each}
</table>
