<script>
	import { onNavigate } from '$app/navigation';
	import { page } from '$app/stores';

	onNavigate((navigation) => {
		if (!document.startViewTransition) return;

		return new Promise((resolve) => {
			const image = /** @type {HTMLElement} */ (
				document.querySelector(`a[href="${navigation.to?.url.pathname}"] *`)
			);

			const vtn = image?.dataset.vtn;

			console.log(image, vtn);

			if (image && vtn) {
				image.style.setProperty('view-transition-name', vtn);
			}

			document.startViewTransition(async () => {
				resolve();
				await navigation.complete;
			});
		});
	});
</script>

<div class="container">
	<nav>
		<ol>
			<li aria-current={$page.url.pathname === '/' ? 'page' : undefined}>
				<a href="/">/</a>
			</li>
			<li aria-current={$page.url.pathname.startsWith('/abc') ? 'page' : undefined}>
				<a href="/abc">abc</a>
			</li>
			<li aria-current={$page.url.pathname.startsWith('/xyz') ? 'page' : undefined}>
				<a href="/xyz">xyz</a>
			</li>
		</ol>
	</nav>

	<main>
		<slot />
	</main>
</div>

<style>
	.container {
		min-height: 100vh;
		display: flex;
		flex-direction: column;
		font-weight: bold;
		font-family: sans-serif;
	}
	main {
		flex: 1;
		padding: 1rem;
	}
	ol {
		list-style: none;
		display: flex;
		gap: 1rem;
		font-size: 2rem;
		padding-inline: 2rem;
	}
	li {
		padding: 1rem;
		position: relative;
	}
	a {
		text-decoration: none;
	}
	li[aria-current]::before {
		content: '';
		position: absolute;
		inset-block-end: 0;
		inset-inline-start: 0;
		width: 100%;
		height: 0.25rem;
		background-color: red;
		view-transition-name: indicator;
	}
</style>
