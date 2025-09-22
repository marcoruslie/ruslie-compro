<template>
	<div class="min-h-screen bg-gray-50 flex flex-col items-center justify-center p-6">
		<!-- Header Section -->
		<header class="text-center mb-10">
			<h1 class="text-4xl font-bold" :style="{ color: primaryColor }">
				Tri Karya Abadi
			</h1>
			<p class="text-gray-600 text-lg mt-2">
				Delivering Excellence, Building Trust
			</p>
		</header>

		<!-- About Section -->
		<section class="max-w-2xl bg-white shadow-lg rounded-2xl p-8 mb-10">
			<h2 class="text-2xl font-semibold mb-4" :style="{ color: primaryColor }">
				About Us
			</h2>
			<p class="text-gray-700 leading-relaxed">
				Tri Karya Abadi is a trusted partner in providing innovative solutions
				and high-quality services to meet the needs of our clients. Our team is
				dedicated to delivering excellence through professionalism, integrity,
				and a commitment to long-term relationships.
			</p>
		</section>

		<!-- Services Section -->
		<section class="max-w-4xl grid grid-cols-1 md:grid-cols-3 gap-6 mb-10">
			<div v-for="service in services" :key="service"
				class="bg-white shadow-md rounded-2xl p-6 text-center hover:shadow-xl transition">
				<h3 class="text-xl font-semibold mb-3" :style="{ color: primaryColor }">
					{{ service }}
				</h3>
				<p class="text-gray-600 text-sm">
					Professional {{ service.toLowerCase() }} services tailored to your needs.
				</p>
			</div>
		</section>

		<!-- Contact Section -->
		<section class="max-w-2xl bg-white shadow-lg rounded-2xl p-8 text-center">
			<h2 class="text-2xl font-semibold mb-4" :style="{ color: primaryColor }">
				Contact Us
			</h2>
			<p class="text-gray-700 mb-4">
				Have a project in mind? We’d love to hear from you.
			</p>
			<a href="mailto:info@trikaryaabadi.com"
				class="inline-block px-6 py-3 rounded-full text-white text-lg shadow-md hover:shadow-lg transition"
				:style="{ backgroundColor: primaryColor }">
				Get in Touch
			</a>
		</section>
	</div>
</template>

<script setup>
const primaryColor = '#033272'
const services = ['Consulting', 'Engineering', 'Project Management']
</script>
