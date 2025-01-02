<script lang="ts">
	import { onMount } from 'svelte';
	import SignaturePad from 'signature_pad';

	let signaturePad: InstanceType<typeof SignaturePad>;
	let canvas: HTMLCanvasElement;
	let devicePixelRatio = $state(1);

	let {
		options = {
			dotSize: 0.5,
			minWidth: 0.5,
			maxWidth: 2.5,
			throttle: 16,
			minDistance: 5,
			backgroundColor: 'rgba(0,0,0,0)',
			penColor: 'black',
			velocityFilterWeight: 0.7
		},
		beginStroke = () => {},
		endStroke = () => {},
		beforeUpdateStroke = () => {},
		afterUpdateStroke = () => {}
	} = $props();

	function handleEndStroke() {
		endStroke();
	}

	function beginStrokeHandler() {
		beginStroke();
	}

	function beforeUpdateStrokeHandler() {
		beforeUpdateStroke();
	}

	function afterUpdateStrokeHandler() {
		afterUpdateStroke();
	}

	export function clear() {
		signaturePad.clear();
	}

	export function getSignature() {
		return signaturePad.toDataURL();
	}

	export function isEmpty() {
		return signaturePad.isEmpty();
	}

	const handleResize = () => {
		const ratio = Math.max(window.devicePixelRatio || 1, 1);
		console.log(ratio);
		if (devicePixelRatio !== ratio) {
			const old_signature = signaturePad.toData();
			devicePixelRatio = ratio;
			const ctx = canvas.getContext('2d');
			const width = canvas.width;
			const height = canvas.height;
			canvas.width = canvas.offsetWidth * ratio;
			canvas.height = canvas.offsetHeight * ratio;
			ctx?.scale(ratio, ratio);
			signaturePad.clear();
			signaturePad.fromData(old_signature);
		}
	};

	onMount(() => {
		signaturePad = new SignaturePad(canvas, options);
		signaturePad.addEventListener('endStroke', handleEndStroke);
		signaturePad.addEventListener('beginStroke', beginStrokeHandler);
		signaturePad.addEventListener('beforeUpdateStroke', beforeUpdateStrokeHandler);
		signaturePad.addEventListener('afterUpdateStroke', afterUpdateStrokeHandler);
		handleResize();
	});
</script>

<canvas bind:this={canvas} onresize={handleResize}></canvas>

<style>
	canvas {
		border: 1px solid black;
		width: 100%;
		height: 100%;
	}
</style>
