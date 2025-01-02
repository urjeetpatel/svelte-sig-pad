<script lang="ts">
	import SigPad from '$lib/SigPad.svelte';
	let data = $state('');
	let height = $state(150);
	let width = $state(600);

	let pad = $state(null);

	function handleEndStroke() {
		data = pad.getSignature();
	}
</script>

<h1>Welcome to your library project</h1>
<p>Create your package using @sveltejs/package and preview/showcase your work with SvelteKit</p>
<p>Visit <a href="https://svelte.dev/docs/kit">svelte.dev/docs/kit</a> to read the documentation</p>
<div style="height: 50vh; width: 50vw; border: 1px solid black;">
	<SigPad bind:this={pad} endStroke={handleEndStroke} />
</div>
<p>{height}</p>

<button
	onclick={() => {
		pad.clear();
	}}
	>Clear
</button>

<pre>
{data}
</pre>
