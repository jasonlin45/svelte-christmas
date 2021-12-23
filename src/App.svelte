<script>
	import { elasticOut, quintOut } from 'svelte/easing';
	import { fade, draw, fly } from 'svelte/transition';
	import { expand } from './custom-transitions.js';
	import { tree, star } from './shape.js';
	import { onMount } from 'svelte';

	let visible = false;
	let replay = false;
	
	function playClick(){
		visible = true;
	}

	function replayClick(){
		visible = false;
		replay = false;
	}

	let ready = false;
	onMount(()=>ready=true)

</script>

{#if ready}
<main transition:fade="{{duration: 1000}}">
	{#if visible}
		<svg width="100%" xmlns="http://www.w3.org/2000/svg" viewBox="75 -5 75 400">
			<g out:fade="{{duration: 200}}" opacity=0.4>
				<path
					in:fade="{{duration: 3000, delay: 3000, easing: quintOut}}"
					style="fill: #FFD600;"
					on:introend="{() => replay = true}"
					d={star}
				/>
				<path
					in:draw="{{duration: 3000, delay: 1000}}"
					style="fill: none; stroke:#FFD600; stroke-width: 1.5;"
					d={star}
				/>
				<path
					in:expand="{{duration: 5000, delay: 1000, easing: quintOut}}"
					style="fill:none; stroke: #52AC62; stroke-width: 10;"
					d={tree}
				></path>
				<path
					in:draw="{{duration: 5000}}"
					style="stroke:#3A7945; stroke-width: 1.5;"
					d={tree}
				/>
			</g>
		</svg>
		
	
		<div class="centered" out:fly="{{y: -20, duration: 800}}">
			<span class='green' in:fly="{{y: -200, duration: 1000}}">
				MERRY
			</span>
			{#each 'CHRISTMAS' as char, i}
				<span
					in:fade="{{delay: 1000 + i * 150, duration: 800}}"
				>{char}</span>
			{/each}
		</div>
		{/if}
	</main>
{/if}

{#if !visible && ready}
	<button out:fade="{{duration: 300}}" in:fly="{{y:-500, x:-200, duration: 2000, easing: elasticOut}}" class="btn center" on:click={playClick}>
		PLAY ▶
	</button>
{/if}
{#if replay}
	<button transition:fade="{{duration: 500}}" class="btn btn2 topleft" on:click={replayClick}>
		REPLAY ↺
	</button>
{/if}
	<!--<input type="checkbox" bind:checked={visible}>
	toggle me-->

<style>
	:global(body) {
		padding: 0px;
	}

	@keyframes fadeIn {
		from {background-color: #ffffff;}
		to {background-color: #52AC6226;}
	}


	main::before {
		position: absolute;
		content: '';
		width: 100%;
		height: 100%;
		background-image: url('/images/snow.gif');
	}

	main {
		width: 100%;
		height: 100%;
		animation-fill-mode: forwards;
		animation: fadeIn 1s ease 1s forwards;
	}

	svg {
		width: 100%;
		height: 100%;
	}

	path {
		fill: white;
		opacity: 1;
	}

	.btn {
		font-family: 'Impact';
		font-size: 20vh;
		color: #1b411d;
		transform: translate(-50%, -50%);
		fill: none;
		background-color: transparent;
		border: 5px solid #411b1b;
		border-radius: 50px;
		transition: background-color 500ms ease;
		transition: font-size 500ms ease-in-out;
	}

	.btn:hover {
		cursor: pointer;
		background-color: #52AC6226;
		font-size: 21vh;
	}

	.green {
		color: #1b411d;

	}

	.btn2 {
		font-size: 4vh;
		transform: translate(0, 0);
	}

	.btn2:hover {
		font-size: 4vh;
	}

	.topleft {
		position: absolute;
		left: 1%;
		top: 1%;
	}

	.center {
		position: absolute;
		left: 50%;
		top: 50%;
	}

	.centered {
		font-size: 10vw;
		position: absolute;
		left: 50%;
		top: 50%;
		transform: translate(-50%,-50%);
		font-family: 'Roboto';
		letter-spacing: 0.12em;
		color: #411b1b;
		font-weight: 400;
		text-align: center;
	}

	.centered span {
		will-change: filter;
	}
</style>