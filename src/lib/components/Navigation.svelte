<script lang="ts">
	import { Home, Search, ListMusic, type Icon } from 'lucide-svelte';
	import type { ComponentType } from 'svelte';
	import logo from '$assets/Spotify_Logo_RGB_White.png';
	import { page } from '$app/stores';

	export let desktop: boolean;

	const menuItems: { path: string; label: string; icon: ComponentType<Icon> }[] = [
		{ path: '/', label: 'Home', icon: Home },
		{ path: '/search', label: 'Search', icon: Search },
		{ path: '/playlists', label: 'Playlists', icon: ListMusic }
	];
</script>

<div class="nav-content" class:desktop class:mobile={!desktop}>
	<nav aria-label="Main">
		<div class="nav-content-inner">
			<img src={logo} alt="Spotify" class="logo" width="100px" />
			<ul>
				{#each menuItems as item}
					<li class:active={item.path === $page.url.pathname}>
						<svelte:component
							this={item.icon}
							focusable="false"
							aria-hidden="true"
							color="var(--text-color)"
							size={26}
							strokeWidth={2}
						/>
						<a href={item.path}>{item.label}</a>
					</li>
				{/each}
			</ul>
		</div>
	</nav>
</div>

<style lang="scss">
	.nav-content {
		.logo {
			max-width: 100%;
			width: 130px;
		}
		.nav-content-inner {
			padding: 20px;
			min-width: var(--sidebar-width);
			background-color: var(--sidebar-color);
			height: 100vh;
			overflow: auto;
			display: none;
			ul {
				padding: 0;
				margin: 20px 0 0;
				list-style: none;
				:global(svg) {
					margin-right: 12px;
				}
				li {
					display: flex;
					align-items: center;
					&.active {
						a {
							opacity: 1;
						}
					}
					a {
						text-decoration: none;
						color: var(--text-color);
						font-size: functions.toRem(14);
						font-weight: 500;
						padding: 4px;
						margin: 10px 0;
						opacity: 0.5;
						transition: opacity 0.2s ease;
						&:hover,
						&:focus {
							opacity: 1;
						}
					}
				}
			}
		}
		&.desktop {
			position: sticky;
			top: 0;
			.nav-content-inner {
				@include breakpoint.up(md) {
					display: block;
				}
			}
		}
	}

	.mobile {
	}
</style>
