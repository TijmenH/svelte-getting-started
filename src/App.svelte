<script>
	import { Route, Router } from 'svelte-routing';

	import Library from './pages/Library.svelte';
	import Detail from './pages/Detail.svelte';
	import Create from './pages/Create.svelte';

	export let url = '';
</script>

<Router {url}>
	<main>
		<Route path="books/create">
			<Create />
		</Route>
		<Route path="books/:id" let:params>
			<Detail id={params.id} />
		</Route>
		<Route path="/">
			<Library />
		</Route>
	</main>
</Router>

<style>
	main {
		padding: var(--spacingLarge);
	}
</style>