<template>
	<div class="min-h-screen bg-gray-50 flex flex-col">
		<!-- Navbar -->
		<Navbar />

		<main class="pt-20">
			<!-- Hero Section -->
			<section
				id="home"
				class="relative w-full max-w-6xl mx-auto mb-12">
				<Swiper
					:modules="[Pagination, Autoplay]"
					:loop="true"
					:autoplay="{ delay: 3500 }"
					:pagination="{ clickable: true }"
					class="rounded-2xl shadow-2xl">
					<SwiperSlide
						v-for="(slide, index) in slides"
						:key="index"
						class="relative">
						<img
							:src="slide.image"
							class="w-full h-[450px] object-cover rounded-2xl" />
						<div
							class="absolute inset-0 bg-black/40 flex flex-col justify-center items-center text-center text-white px-4 animate-fade-in">
							<h2 class="text-4xl font-extrabold drop-shadow-lg animate-slide-up">
								{{ slide.title }}
							</h2>
							<p class="text-lg mt-2 max-w-xl animate-fade-in-delayed">
								{{ slide.subtitle }}
							</p>
						</div>
					</SwiperSlide>
				</Swiper>
			</section>

			<!-- Services -->
			<section
				id="services"
				class="max-w-6xl mx-auto text-center mb-16">
				<h2
					class="text-3xl font-bold mb-8 animate-slide-up"
					:style="{ color: primaryColor }">
					Layanan Kami
				</h2>
				<p class="max-w-2xl mx-auto text-gray-600 mb-10 animate-fade-in">
					Kami menerima pembuatan berbagai macam spring (pegas) dengan diameter kawat mulai dari
					<strong>0.3mm hingga 40mm</strong>. Cocok untuk industri otomotif, elektronik, furniture, dan
					kebutuhan custom.
				</p>
				<div class="grid grid-cols-1 md:grid-cols-3 gap-6">
					<div
						v-for="service in services"
						:key="service.title"
						class="relative h-80 rounded-2xl overflow-hidden group cursor-pointer shadow-lg">
						<img
							:src="service.image"
							class="absolute inset-0 w-full h-full object-cover transition-transform duration-700 group-hover:scale-125" />

						<div
							class="absolute inset-0 bg-gradient-to-t from-black/80 via-black/40 to-transparent hover:opacity-100 transition-opacity duration-300"></div>

						<div class="absolute bottom-0 p-6">
							<h3
								class="text-white text-2xl font-bold mb-1 translate-y-4 group-hover:translate-y-0 transition-transform duration-300">
								{{ service.title }}
							</h3>
							<p
								class="text-gray-300 text-sm opacity-0 group-hover:opacity-100 transition-opacity duration-300">
								{{ service.desc }}
							</p>
						</div>
					</div>
				</div>
			</section>

			<!-- Gallery -->
			<section
				id="gallery"
				class="py-10">
				<div class="max-w-6xl mx-auto mb-16">
					<h2
						class="text-3xl font-bold text-center mb-8 animate-slide-up"
						:style="{ color: primaryColor }">
						Galeri Produksi & Contoh Produk
					</h2>
					<div class="columns-1 md:columns-4 gap-4 space-y-4">
						<div
							v-for="item in gallery"
							:key="item"
							class="break-inside-avoid overflow-hidden rounded-xl shadow-md hover:scale-110 hover:z-50 transition relative group"
							@click="openModal(item)">
							<img
								:src="item"
								class="w-full h-auto object-contain" />
						</div>
					</div>
				</div>

				<!-- Modal Zoom -->
				<div
					v-if="selectedImage"
					class="fixed inset-0 bg-black/70 flex justify-center items-center z-50">
					<div class="relative max-w-3xl">
						<img
							:src="selectedImage"
							class="rounded-xl shadow-2xl max-h-[80vh]" />
						<button
							@click="selectedImage = null"
							class="absolute top-2 right-2 bg-white rounded-full p-2 shadow hover:scale-110 transition">
							✕
						</button>
					</div>
				</div>
			</section>
			<section
				id="video-showcase"
				class="py-16 bg-gray-50">
				<div class="max-w-6xl mx-auto px-4 text-center">
					<h2
						class="text-3xl font-bold mb-4 animate-slide-up"
						:style="{ color: primaryColor }">
						Lihat Proses Produksi Kami
					</h2>
					<p class="text-gray-600 mb-10 max-w-2xl mx-auto">
						Saksikan bagaimana dedikasi dan teknologi mesin presisi kami bekerja untuk menghasilkan produk
						pegas berkualitas tinggi.
					</p>

					<div class="relative max-w-sm mx-auto overflow-hidden rounded-2xl shadow-2xl bg-black">
						<video
							controls
							class="w-full h-auto aspect-[9/16] object-cover"
							poster="/images/Mesin1.jpeg">
							<source
								src="/images/Mesin1Vid.mp4"
								type="video/mp4" />
							Browser Anda tidak mendukung tag video.
						</video>
					</div>

					<div
						class="mt-8 flex justify-center gap-8 text-sm font-semibold text-gray-500 uppercase tracking-widest">
						<span class="flex items-center gap-2">⚙️ Automasi Tinggi</span>
						<span class="flex items-center gap-2">🔍 Quality Control</span>
						<span class="flex items-center gap-2">🚀 Pengiriman Cepat</span>
					</div>
				</div>
			</section>

			<section
				id="partners"
				class="py-12 bg-gray-100/50 mb-16">
				<div class="max-w-6xl mx-auto px-4 text-center">
					<h2 class="text-2xl font-bold mb-10 text-gray-400 uppercase tracking-widest">
						Telah Dipercaya Oleh
					</h2>

					<div class="relative overflow-hidden py-10">
						<div class="flex animate-scroll whitespace-nowrap">
							<div
								class="flex space-x-12 items-center mx-6"
								v-for="i in 2"
								:key="i">
								<img
									v-for="p in partners"
									:src="p.logo"
									:alt="p.name"
									class="h-20 grayscale duration-300 ease-out hover:scale-125 hover:grayscale-0 rounded-xl transition" />
							</div>
						</div>
					</div>
				</div>
			</section>
			<!-- About -->
			<section
				id="about"
				class="max-w-4xl mx-auto bg-white shadow-xl rounded-2xl p-8 mb-16 animate-slide-up">
				<h2
					class="text-3xl font-semibold mb-4"
					:style="{ color: primaryColor }">
					Tentang Kami
				</h2>
				<p class="text-gray-700 leading-relaxed">
					Ruslie Spring adalah mitra terpercaya dalam penyediaan solusi pegas dan wire forming untuk berbagai
					kebutuhan industri. Dengan peralatan modern, tim profesional, dan standar kualitas yang ketat, kami
					selalu memastikan produk sesuai dengan spesifikasi yang Anda butuhkan.
				</p>
			</section>

			<!-- Lokasi -->
			<section class="max-w-5xl mx-auto px-6 py-16">
				<h2 class="text-3xl font-bold text-center text-[#033272] mb-8">Lokasi Kami</h2>
				<iframe
					class="w-full h-80 rounded-xl shadow-lg"
					src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3957.815738966079!2d112.67010927499967!3d-7.261800192744962!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x2dd7fe8612b51cf5%3A0x213422d4b5a515aa!2sJl.%20Sikatan%20No.45%2C%20Manukan%20Wetan%2C%20Kec.%20Tandes%2C%20Surabaya%2C%20Jawa%20Timur%2060185!5e0!3m2!1sen!2sid!4v1758535029271!5m2!1sen!2sid"
					allowfullscreen=""
					loading="lazy"></iframe>
			</section>
		</main>
		<!-- Footer Section -->
		<footer class="bg-[#021d47] text-white mt-16">
			<div class="max-w-6xl mx-auto px-6 py-12 grid grid-cols-1 md:grid-cols-3 gap-8">
				<div>
					<h3 class="text-xl font-bold mb-4">Ruslie Spring</h3>
					<p class="text-gray-300">
						Spesialis spring & wire forming custom. Kami siap melayani kebutuhan industri Anda.
					</p>
				</div>
				<div>
					<h3 class="text-xl font-bold mb-4">Contact Person</h3>
					<div
						v-for="person in contactPersons"
						:key="person.name"
						class="mb-3">
						<p class="font-semibold">{{ person.name }}</p>
						<p class="text-gray-300 text-sm">{{ person.role }}</p>
						<a
							:href="`https://wa.me/${person.whatsapp}`"
							target="_blank"
							class="text-green-400 hover:underline">
							📱 {{ person.whatsapp }}
						</a>
					</div>
				</div>
				<div>
					<h3 class="text-xl font-bold mb-4">Navigasi</h3>
					<ul class="space-y-2 text-gray-300">
						<li>
							<a
								href="#home"
								class="hover:text-white"
								>Home</a
							>
						</li>
						<li>
							<a
								href="#products"
								class="hover:text-white"
								>Produk</a
							>
						</li>
						<li>
							<a
								href="#gallery"
								class="hover:text-white"
								>Galeri</a
							>
						</li>
						<li>
							<a
								href="#about"
								class="hover:text-white"
								>Tentang</a
							>
						</li>
						<li>
							<a
								href="#contact"
								class="hover:text-white"
								>Kontak</a
							>
						</li>
					</ul>
				</div>
			</div>
			<div class="text-center text-gray-400 py-4 text-sm border-t border-gray-600">
				© {{ new Date().getFullYear() }} Ruslie Spring. All rights reserved.
			</div>
		</footer>
		<!-- Floating WhatsApp Button -->
		<a
			href="https://wa.me/6285104815151"
			target="_blank"
			class="fixed bottom-6 right-6 bg-green-500 hover:bg-green-600 text-white p-4 rounded-full shadow-lg">
			💬
		</a>
	</div>
</template>

<script setup>
	import { Swiper, SwiperSlide } from "swiper/vue"
	import { Pagination, Autoplay } from "swiper/modules"
	import "swiper/css"
	import "swiper/css/pagination"
	import Navbar from "~/components/navbar.vue"

	const route = useRoute()
	const mobileMenu = ref(false)

	const primaryColor = "#033272"
	const whatsappNumber = "6285104815151"

	const selectedImage = ref(null)
	function openModal(img) {
		selectedImage.value = img
	}

	const slides = [
		{
			image: "/images/banner1.jpg",
			title: "Spesialis Pembuatan Spring",
			subtitle: "Terima segala macam spring mulai dari 0.3mm - 40mm",
		},
		{
			image: "/images/banner2.jpg",
			title: "Produksi Modern & Presisi",
			subtitle: "Menggunakan mesin canggih untuk hasil terbaik",
		},
		{
			image: "/images/banner3.jpg",
			title: "Solusi Custom untuk Industri",
			subtitle: "Kami siap mendukung kebutuhan unik Anda",
		},
	]

	const services = [
		{
			title: "Spring Compression",
			desc: "Pegas tekan presisi tinggi yang dirancang khusus untuk performa maksimal pada mesin industri, sistem otomotif, dan perangkat elektronik.",
			image: "/images/gallery4.jpg",
		},
		{
			title: "Spring Tension",
			desc: "Pegas tarik dengan daya tahan unggul, memastikan elastisitas dan kekuatan tarikan yang konsisten untuk berbagai aplikasi mekanikal.",
			image: "/images/gallery2.jpg",
		},
		{
			title: "Custom Wire Forming",
			desc: "Solusi pembentukan kawat kustom dengan akurasi tinggi, disesuaikan sepenuhnya dengan spesifikasi desain dan kebutuhan teknis proyek Anda.",
			image: "/images/gallery3.jpg",
		},
	]

	const partners = [
		{ name: "Maspion", logo: "/logos/maspion.png" },
		{ name: "Polytron", logo: "/logos/polytron.jpg" },
		{ name: "Santini", logo: "/logos/santini.jpg" },
		// { name: "Singa Oak", logo: "/logos/singa-oak.png" },
	]
	const gallery = [
		"/images/item1.jpg",
		"/images/gallery1.jpg",
		"/images/item3.jpg",
		"/images/item4.jpg",
		"/images/gallery5.jpg",
		"/images/item5.jpg",
		"/images/gallery6.jpg",
		"/images/item6.png",
		"/images/item2.jpg",
		"/images/gallery7.jpg",
		"/images/Mesin1.jpeg",
	]
	const contactPersons = [{ name: "Marco Ruslie", role: "", whatsapp: "6285104815151" }]
</script>

<style>
	html {
		scroll-behavior: smooth;
	}

	/* Animasi */
	@keyframes fade-in {
		from {
			opacity: 0;
		}

		to {
			opacity: 1;
		}
	}

	@keyframes slide-up {
		from {
			opacity: 0;
			transform: translateY(30px);
		}

		to {
			opacity: 1;
			transform: translateY(0);
		}
	}

	@keyframes zoom-in {
		from {
			opacity: 0;
			transform: scale(0.9);
		}

		to {
			opacity: 1;
			transform: scale(1);
		}
	}

	.animate-fade-in {
		animation: fade-in 1s ease forwards;
	}

	.animate-slide-up {
		animation: slide-up 1s ease forwards;
	}

	.animate-zoom-in {
		animation: zoom-in 0.8s ease forwards;
	}

	.animate-fade-in-delayed {
		animation: fade-in 1.2s ease forwards;
	}
	@keyframes scroll {
		0% {
			transform: translateX(0);
		}
		100% {
			transform: translateX(-50%);
		}
	}

	.animate-scroll {
		display: flex;
		width: max-content;
		animation: scroll 30s linear infinite;
	}

	.animate-scroll:hover {
		animation-play-state: paused;
	}
</style>
