<script lang="ts">
	import { PUBLIC_DEFAULT_HEADER_AUTHORIZATION, PUBLIC_DEFAULT_URL } from '$env/static/public';

	let url = PUBLIC_DEFAULT_URL;
	let headerAuthorization = PUBLIC_DEFAULT_HEADER_AUTHORIZATION;
	let error: Error | undefined = undefined;

	let responseBody: string;

	const post = async () => {
		try {
			const response = await fetch(url, {
				method: 'POST',
				headers: {
					'Content-Type': 'application/json',
					Authorization: headerAuthorization
				}
			});

			responseBody = await response.json();
			return {
				status: response.status,
				body: await response.json()
			};
		} catch (err) {
			if (err instanceof Error) error = err;
			console.log(err);
		}
	};

	// onMount(() => {
	// 	setInterval(() => {
	// 		error = undefined;
	// 		post();
	// 	}, 5000);
	// });
</script>

<h1>Cors tester</h1>

<form action={url} method="POST" on:submit|preventDefault={() => post()}>
	{url}
	<label>
		<div>Url (method: post)</div>
		<input type="text" bind:value={url} />
	</label>

	<label>
		<div>Authorization header</div>
		<input type="text" bind:value={headerAuthorization} />
	</label>

	<button type="submit">Post</button>
</form>

{#if responseBody}
	<pre>{JSON.stringify(responseBody, null, 2)}</pre>
{/if}

{#if error}
	<pre>{error.message}</pre>
	<p>See browser console for details</p>
{/if}

<style>
	input {
		width: 100%;
	}

	form {
		display: grid;
		gap: 1em;
	}
</style>
