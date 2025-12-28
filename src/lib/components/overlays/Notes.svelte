<script lang="ts">
	import { Button } from '$lib/components/ui/button';
	import { Card, CardContent, CardHeader, CardTitle } from '$lib/components/ui/card';
	import {
		Collapsible,
		CollapsibleContent,
		CollapsibleTrigger
	} from '$lib/components/ui/collapsible';
	import { Separator } from '$lib/components/ui/separator';

	import { StickyNote, X } from 'lucide-svelte';

	// 1) Load markdown as raw text at build time
	import notesMd from '$lib/notes/first.md?raw';

	// 2) Optional: markdown -> HTML (recommended)
	// bun add marked
	import { marked } from 'marked';

	let open = $state(false);

	const html = $derived(marked.parse(notesMd));
</script>

<div class="pointer-events-auto absolute right-3 bottom-3 z-20">
	<Collapsible bind:open>
		<div class="flex justify-end">
			<CollapsibleTrigger>
				<Button
					variant="secondary"
					size="icon"
					class="bg-background/80 shadow-md backdrop-blur supports-[backdrop-filter]:bg-background/60"
					aria-label={open ? 'Close notes' : 'Open notes'}
				>
					{#if open}
						<X class="h-4 w-4" />
					{:else}
						<StickyNote class="h-4 w-4" />
					{/if}
				</Button>
			</CollapsibleTrigger>
		</div>

		<CollapsibleContent>
			<Card
				class="mt-3 w-[360px] bg-background/80 shadow-lg backdrop-blur supports-[backdrop-filter]:bg-background/60"
			>
				<CardHeader class="py-3">
					<CardTitle class="text-sm">Notes</CardTitle>
				</CardHeader>

				<CardContent class="space-y-3">
					<Separator />

					<!-- Tailwind typography -->
					<div class="prose prose-sm dark:prose-invert max-h-[60vh] max-w-none overflow-auto pr-1">
						{@html html}
					</div>
				</CardContent>
			</Card>
		</CollapsibleContent>
	</Collapsible>
</div>
