<script lang="ts">
	import DocsShell from '$docs/layouts/DocsShell/DocsShell.svelte';
	import { DocsFeature, type DocsShellSettings } from '$docs/layouts/DocsShell/types';
	import DocsPreview from '$docs/components/DocsPreview/DocsPreview.svelte';
	// Components
	import CodeBlock from '$lib/utilities/CodeBlock/CodeBlock.svelte';
	import ConicGradient from '$lib/components/ConicGradient/ConicGradient.svelte';
	import type { ConicStop } from '$lib/components/ConicGradient/types';
	import TabGroup from '$lib/components/Tab/TabGroup.svelte';
	import Tab from '$lib/components/Tab/Tab.svelte';
	// Sveld
	import sveldConicGradient from '$lib/components/ConicGradient/ConicGradient.svelte?raw&sveld';

	// Docs Shell
	const settings: DocsShellSettings = {
		feature: DocsFeature.Component,
		name: 'Conic Gradients',
		description: 'Create conic gradient data visualizations for pie charts, loading spinners, and more.',
		imports: ['ConicGradient'],
		types: ['ConicStop'],
		source: 'components/ConicGradient',
		components: [{ sveld: sveldConicGradient }]
	};

	// Local
	let tabColors: number = 0;

	// Color Stops
	let stopsDemo: ConicStop[] = [
		{ color: 'rgba(255,255,255,1)', start: 0, end: 10 },
		{ color: 'rgba(255,255,255,0.5)', start: 10, end: 35 },
		{ color: 'rgba(255,255,255,0.25)', start: 35, end: 100 }
	];
	let stopsLegend: ConicStop[] = [
		{ label: 'Orange', color: ['orange', 500], start: 0, end: 10 },
		{ label: 'Yellow', color: ['yellow', 500], start: 10, end: 35 },
		{ label: 'Red', color: ['red', 500], start: 35, end: 100 }
	];
	const stopsSpinner: ConicStop[] = [
		{ color: 'transparent', start: 0, end: 25 },
		{ color: 'rgb(var(--color-primary-500))', start: 75, end: 100 }
	];
</script>

<DocsShell {settings}>
	<!-- Slot: Sandbox -->
	<svelte:fragment slot="sandbox">
		<DocsPreview>
			<svelte:fragment slot="preview">
				<ConicGradient stops={stopsDemo} width="w-48" />
			</svelte:fragment>
			<svelte:fragment slot="source">
				<CodeBlock
					language="ts"
					code={`
const conicStops: ConicStop[] = [
	{ color: 'rgba(255,255,255,1)', start: 0, end: 10 },
	{ color: 'rgba(255,255,255,0.5)', start: 10, end: 35 },
	{ color: 'rgba(255,255,255,0.25)', start: 35, end: 100 }
];
`}
				/>
				<CodeBlock language="html" code={`<ConicGradient stops={conicStops}>(caption)</ConicGradient>`} />
			</svelte:fragment>
		</DocsPreview>
	</svelte:fragment>

	<!-- Slot: Usage -->
	<svelte:fragment slot="usage">
		<!-- Color Stops -->
		<div class="space-y-4">
			<p>
				Provide one or more color stops that start with values that range from <code>0%</code> to <code>100%</code>. A stop of
				<code>0%</code> starts at the top middle, then additional values are added in a clock-wise direction.
			</p>
			<TabGroup regionPanel="space-y-4">
				<Tab bind:group={tabColors} name="tab1" value={0}>Theme Colors</Tab>
				<Tab bind:group={tabColors} name="tab2" value={1}>Tailwind Colors</Tab>
				<Tab bind:group={tabColors} name="tab3" value={2}>Custom Colors</Tab>
				<!-- Tab Panels --->
				<svelte:fragment slot="panel">
					{#if tabColors === 0}
						<!-- Theme Colors --->
						<p>Provide a theme color CSS custom property <code>var(--color-primary-500)</code> wrapped in <code>rgb()</code>.</p>
						<CodeBlock
							language="ts"
							code={`
const conicStops: ConicStop[] = [
	{ label: 'Primary', color: 'rgb(var(--color-primary-500))', start: 0, end: 33 },
	{ label: 'Secondary', color: 'rgb(var(--color-secondary-500))', start: 33, end: 66 },
	{ label: 'Tertiary', color: 'rgb(var(--color-tertiary-500))', start: 66, end: 100 }
];
`}
						/>
					{:else if tabColors === 1}
						<!-- Tailwind Colors -->
						<p>To utilize default Tailwind colors, supply an array with the format <code>[name: string, shade: number]</code>.</p>
						<CodeBlock
							language="ts"
							code={`
const conicStops: ConicStop[] = [
	{ label: 'Orange', color: ['orange', 500], start: 0, end: 10 },
	{ label: 'Yellow', color: ['yellow', 500], start: 10, end: 35 },
	{ label: 'Red', color: ['red', 500], start: 35, end: 100 }
];
`}
						/>
					{:else if tabColors === 2}
						<!-- Custom Colors -->
						<p>
							You can provide <a href="https://developer.mozilla.org/en-US/docs/Web/CSS/color_value" target="_blank" rel="noreferrer"
								>standard CSS color values</a
							> as a string, including: color names, hex, rgba, HSL, or similar.
						</p>
						<CodeBlock
							language="ts"
							code={`
const conicStops: ConicStop[] = [
	{ label: 'Name', color: 'orange', start: 0, end: 10 },
	{ label: 'HSL', color: 'hsl(60deg 100% 50%)', start: 10, end: 35 },
	{ label: 'Hex', color: '#bada55', start: 35, end: 100 }
];
`}
						/>
					{/if}
				</svelte:fragment>
			</TabGroup>
			<!-- prettier-ignore -->
			<p>This data can be reactive, but be sure to adhere to standard Svelte requirements when <a href="https://svelte.dev/tutorial/updating-arrays-and-objects" target="_blank" rel="noreferrer">updating arrays</a>.</p>
		</div>
		<!-- Legend -->
		<div class="space-y-4">
			<h2>Legend</h2>
			<p>By enabling the <code>legend</code> property, a small table of values will appear below the conic gradient.</p>
			<DocsPreview background="neutral">
				<svelte:fragment slot="preview">
					<ConicGradient stops={stopsLegend} legend>Heat Map</ConicGradient>
				</svelte:fragment>
				<svelte:fragment slot="source">
					<CodeBlock
						language="ts"
						code={`
const conicStops: ConicStop[] = [
		{ label: 'One', color: 'rgba(255,255,255,1)', start: 0, end: 10 },
		{ label: 'Two', color: 'rgba(255,255,255,0.5)', start: 10, end: 35 },
		{ label: 'Three', color: 'rgba(255,255,255,0.25)', start: 35, end: 100 }
];
`}
					/>
					<CodeBlock language="html" code={`<ConicGradient stops={conicStops} legend>(caption)</ConicGradient>`} />
				</svelte:fragment>
			</DocsPreview>
		</div>
		<!-- Animated Spinner -->
		<div class="space-y-4">
			<h2>Animated Spinner</h2>
			<p>
				Add the <code>spin</code> property to implement a spinning animation automatically. Try pairing with a smooth gradient by providing gaps
				between when a color stop ends and when the next begins. This will automatically blend.
			</p>
			<DocsPreview background="neutral">
				<svelte:fragment slot="preview">
					<ConicGradient stops={stopsSpinner} spin width="w-8">
						<p class="text-token">Loading</p>
					</ConicGradient>
				</svelte:fragment>
				<svelte:fragment slot="source">
					<CodeBlock
						language="ts"
						code={`
const conicStops: ConicStop[] = [
	{ color: 'transparent', start: 0, end: 25 },
	{ color: 'rgb(var(--color-primary-500))', start: 75, end: 100 }
];
`}
					/>
					<CodeBlock language="html" code={`<ConicGradient stops={conicStops} spin>Loading</ConicGradient>`} />
				</svelte:fragment>
			</DocsPreview>
		</div>
		<!-- Rotate Axis -->
		<div class="space-y-4">
			<h2>Rotate Axis</h2>
			<p>
				Use the <a href="https://tailwindcss.com/docs/rotate" target="_blank" rel="noreferrer">Tailwind rotate</a> utility clases with the
				<code>regionCone</code> property to change the starting axis position.
			</p>
			<CodeBlock language="html" code={`<ConicGradient ... regionCone="rotate-90" />`} />
		</div>
	</svelte:fragment>
</DocsShell>
