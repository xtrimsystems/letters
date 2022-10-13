<script lang="ts">
	import { configurationStore } from './Stores/ConfigurationStore';
	import { customCategoriesStore } from './Stores/CustomCategoriesStore';
	import { customStagesStore } from './Stores/CustomStagesStore';
	import { Categories } from './Categories';
	import type { I18nCategories } from './Domain';
	import type { Stage } from './Domain/Stage'
	import type { Category } from './Domain/Category'

	export let stages: Stage[];
	let category: Category;
	let categories: Category[] = [];
	let i18nCategories: I18nCategories;

	$: i18nCategories = Categories.find((c) => $configurationStore.language.match(c.language));
	$: if (i18nCategories) categories = [...$customCategoriesStore, ...i18nCategories.categories];
	$: if (category) stages = [
			...i18nCategories.stages.filter((stage: Stage) => stage.categoryId === category.id),
			...$customStagesStore.filter((stage: Stage) => stage.categoryId === category.id)
	];
	$: console.log(categories);
</script>

<div class="row row-cols-1 row-cols-md-2">
	{#each categories as cat}
	<div class="col mb-4">
		<label for="input-category-{cat.id}" style="cursor:pointer">
			<div class="card">
				<img src="{cat.imageBase64}" class="card-img-top" alt="{cat.name}">
				<div class="card-body">
					<h5 class="card-title">{cat.name}</h5>
				</div>
				<input bind:group={category} type="radio" class="visibleButHidden" value="{cat}" id="input-category-{cat.id}">
			</div>
		</label>
	</div>
	{/each}
</div>
