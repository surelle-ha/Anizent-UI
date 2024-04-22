<template>
	<section class="landing-hero py-8 px-5 lg:px-8">
		<div class="flex flex-wrap">
			<div
				class="w-full xl:w-6 flex flex-column justify-content-center lg:pr-8 align-items-center xl:align-items-stretch"
			>
				<Badge style="width: 350px">🎉 No Ads, No fee & No Clickbait</Badge>
				<h1 class="text-6xl font-bold text-center xl:text-left">
					Watch Your Favorite Show @
					<span class="font-bold text-primary">ANIZENT</span>
				</h1>
				<p class="section-detail xl:text-left text-center px-0 mt-0 mb-5">
					Dive into the ultimate anime experience! Explore endless
					adventures and connect with fellow anime enthusiasts. Whether
					you're into classic hits or the latest releases, our community is
					the perfect place to share your passion and discover new
					favorites!
				</p>
				<div class="flex align-items-center gap-3">
					<Button label="Get Started"
						>Get Started <i class="pi pi-arrow-right ml-3"></i
					></Button>
					<Button label="Give a Star" outlined
						>Give a Star <i class="pi pi-star-fill ml-3 text-yellow-500"></i
					></Button>
				</div>
			</div>

			<div
				class="w-full xl:w-6 flex flex-column justify-content-center align-items-center xl:align-items-stretch"
			>
				<Image
					preview
					alt="user header"
					width="100%"
					src="https://a.storyblok.com/f/178900/1600x900/a618df43a1/37524a216449780d96e12b54024063221677931315_main.png/m/filters:quality(95)format(webp)"
				/>
			</div>
		</div>
	</section>

	<div class="container px-6 py-4">
		<div class="row">
			<div class="col-md-6">
				<NewEpisode />
			</div>

			<div class="container py-4">
				<div class="surface-section px-4 py-8 md:px-6 lg:px-8">
					<div class="text-700 text-center">
						<div class="text-blue-600 font-bold mb-3">
							<i class="pi pi-dev"></i>&nbsp;POWERED BY
							<span style="color: orangered">ZENTINEL</span>
						</div>
						<div
							class="text-900 font-bold text-6xl mb-3"
							style="font-family: 'Jersey 10', sans-serif; letter-spacing: 5px"
						>
							<span style="color: violet">ANI</span>ZENT
						</div>
						<div
							class="text-500 text-2xl mb-5"
							style="
								font-family: 'Signika Negative', sans-serif;
								letter-spacing: 2px;
							"
						>
							Dive into the ultimate anime experience! Explore endless
							adventures and connect with fellow anime enthusiasts. Whether
							you're into classic hits or the latest releases, our community is
							the perfect place to share your passion and discover new
							favorites!
						</div>
						<Register />
					</div>
				</div>
			</div>

			<div class="col-md-6">
				<TopAnime />
			</div>

		</div>
	</div>
	<SpeedDial v-if="false"/>
</template>

<script setup>
import Register from "@/components/Register.vue";
import TopAnime from "@/components/TopAnimeCarousel.vue";
import NewEpisode from "@/components/NewEpisodeCarousel.vue";
import SeriesList from "@/components/SeriesList.vue";
import SpeedDial from "@/components/SpeedDial.vue";
</script>

<style></style>
