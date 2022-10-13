<script lang="ts">
	import { customCategoriesStore } from './Stores/CustomCategoriesStore';
	import CustomCategory from './CustomCategory.svelte';
</script>

<div class="accordion" id="categoriesAccordion">
{#each $customCategoriesStore as category}
    <CustomCategory bind:category="{category}" />
{/each}
</div>
