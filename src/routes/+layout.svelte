<script lang="ts">
	import 'modern-normalize/modern-normalize.css';
	import '$styles/main.scss';
	import type { LayoutData } from './$types';
	import { Navigation } from '$components';

	let topbar: HTMLElement;
	let scrollY: number;
	let headerOpacity = 0;
	$: if (topbar) {
		headerOpacity = scrollY / topbar.offsetHeight < 1 ? scrollY / topbar.offsetHeight : 1;
	}

	export let data: LayoutData;

	$: user = data.user;
</script>

<svelte:window bind:scrollY />

<div class="main">
	{#if user}
		<div class="sidebar">
			<Navigation desktop={true} />
		</div>
	{/if}
	<div class="content">
		<div class="top-bar" bind:this={topbar}>
			top-bar
			<div
				class="top-bar-bg"
				style:background-color="var(--header-color)"
				style:opacity={headerOpacity}
			/>
		</div>
		<main class="main-content" class:logged-in={user}>
			<slot />
		</main>
	</div>
</div>

<style lang="scss">
	.main {
		display: flex;
		.content {
			flex: 1;
			.top-bar {
				height: var(--header-height);
				position: fixed;
				padding: 0 15px;
				display: flex;
				align-items: center;
				width: 100%;
				z-index: 100;
				.top-bar-bg {
					position: absolute;
					width: 100%;
					height: 100%;
					top: 0;
					left: 0;
					z-index: -1;
				}
				@include breakpoint.up(md) {
					padding: 0 30px;
					width: calc(100% - var(--sidebar-width));
				}
			}
			.main-content {
				padding: 30px 15px 60px;
				@include breakpoint.up(md) {
					padding: 30px 30px 60px;
				}
				&.logged-in {
					padding-top: calc(var(--header-height) + 30px);
				}
			}
		}
	}
</style>
