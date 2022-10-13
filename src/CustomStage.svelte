<script lang="ts">
	import { deleteStage, updateStage } from "./Writers/StagesWriter";
	import { ImageFileToBase64 } from './Helpers/ImageFileToBase64'
	import type { Stage } from './Domain';

	export let stage: Stage;
	let isEditable: boolean = false;
	let stageWordBeforeChange: string;
	let stageImageBeforeChange: string;
	let editStageFormFiles: FileList;

	$: (async function imgToBase64() {
		if (editStageFormFiles && editStageFormFiles[0] && editStageFormFiles[0].type.includes('image')) {
			stage.imageBase64 = await ImageFileToBase64.convert(editStageFormFiles[0])
		}
	})();

	async function onClickDeleteStage () {
		await deleteStage(stage);
	}

	async function onClickUpdateStage() {
		if (await updateStage(stage)) {
			isEditable = false;
		}
	}

	function onClickEditStage() {
		stageWordBeforeChange = stage.word;
		stageImageBeforeChange = stage.imageBase64;
		isEditable = true
	}

	function onClickCancelEditStage () {
		stage.word = stageWordBeforeChange;
		stage.imageBase64 = stageImageBeforeChange;
		isEditable = false;
	}
</script>

<li class="list-group-item">
	{#if isEditable}
		<label>
			<img src="{stage.imageBase64}" alt="{stage.word}" class="thumbnail" />
			<input bind:files={editStageFormFiles} class="visibleButHidden" type="file" accept="image/*" />
		</label>
	{:else}
		<img src="{stage.imageBase64}" alt="{stage.word}" class="thumbnail" />
	{/if}
	<input
		bind:value={stage.word}
		readonly="{isEditable}"
		class="form-control form-control-lg"
		type="text"
		placeholder="{stage.word}"
	>
	<div class="cta">
		{#if isEditable}
			<span on:click={onClickDeleteStage}><i class="far fa-trash-alt"></i></span>
			<span class="action" on:click={onClickCancelEditStage}><i class="fas fa-times"></i></span>
			<span class="action" on:click={onClickUpdateStage}><i class="fas fa-check"></i></span>
		{:else}
			<span on:click={onClickEditStage}><i class="far fa-edit"></i></span>
		{/if}
	</div>
</li>
