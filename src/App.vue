<template>
	<div
		class="bg-bluegray-900 text-gray-100 py-1 px-6 flex justify-content-between lg:justify-content-center align-items-center flex-wrap"
		style="margin: -8px; font-size: 12px"
		v-if="false"
	>
		<div class="font-bold mr-8">🔥 Development</div>
		<div class="align-items-center hidden lg:flex">
			<span class="line-height-3"
				>I'm continuously working on this project.</span
			>
		</div>
		<a class="flex align-items-center ml-2 mr-8">
			<a class="text-white" href="#"
				><span class="underline font-bold mr-2">Learn More</span></a
			>
		</a>
		<a
			v-ripple
			class="flex align-items-center no-underline justify-content-center border-circle text-gray-50 hover:bg-bluegray-700 cursor-pointer transition-colors transition-duration-150 p-ripple"
			style="width: 2rem; height: 2rem"
		>
			<i class="pi pi-times"></i>
		</a>
	</div>

	<Toast />

	<div class="container px-6 py-4">
		<Topbar />
	</div>

	<router-view />

  <Footer/>
  
  <particles-bg type="thick" :canvas="{backgroundColor:'#303030'}" :bg="true"/>
</template>

<script setup>
import Topbar from "@/components/Topbar.vue";
import Footer from "@/components/Footer.vue";
import { ParticlesBg } from "particles-bg-vue";
</script>
