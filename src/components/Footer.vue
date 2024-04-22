<template>
	<div class="container mx-6">

		<Toolbar :pt="{
			root: {
				style: 'background-color: rgb(30,30,30,.5)',
				class: 'border-none'
			}
		}">
			<template #start>
				<Button icon="pi pi-whatsapp" label="Contact Developer	" severity="primary"/>
			</template>

			<template #center>
				Powered by Zentinel @ 2024
			</template>

			<template #end> 
				<Button icon="pi pi-facebook" class="mr-2" severity="secondary" />
				<Button icon="pi pi-twitter" class="mr-2" severity="secondary" />
				<Button icon="pi pi-youtube" severity="secondary" />
			</template>
		</Toolbar>

	</div>
</template>

<script setup></script>

<style lang="scss" scoped></style>
