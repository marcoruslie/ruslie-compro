<template>
	<!-- Navbar -->
	<nav class="fixed top-0 w-full z-50 bg-white/80 backdrop-blur-lg shadow-md transition-all duration-300">
		<div class="max-w-7xl mx-auto flex justify-between items-center px-6 md:px-10 py-3">
			<!-- Left: Logo + Brand -->
			<div class="flex items-center gap-3">
				<img
					src="/Logo_Ruslie_Spring.png"
					alt="Ruslie Spring Logo"
					class="w-12 h-12 md:w-14 md:h-14 object-contain rounded-full border border-blue-100 shadow-sm" />
				<div>
					<h1 class="text-xl md:text-2xl font-extrabold text-[#033272] tracking-tight">Ruslie Spring</h1>
					<!-- <p class="text-xs text-gray-500 font-medium uppercase tracking-wide">
						Ruslie Group
					</p> -->
				</div>
			</div>

			<!-- Right: Navigation Links -->
			<ul class="hidden md:flex gap-8 text-gray-700 font-medium items-center">
				<li>
					<NuxtLink
						to="/"
						class="nav-link hover:text-blue-700 transition"
						:class="{
							'text-blue-700 font-semibold border-b-2 border-blue-700 pb-1': route.fullPath === '/',
						}">
						Home</NuxtLink
					>
				</li>
				<li>
					<a
						href="#products"
						class="nav-link hover:text-blue-700 transition"
						:class="{
							'text-blue-700 font-semibold border-b-2 border-blue-700 pb-1':
								route.fullPath === '/#products',
						}"
						>Produk</a
					>
				</li>
				<li>
					<a
						href="#gallery"
						class="nav-link hover:text-blue-700 transition"
						:class="{
							'text-blue-700 font-semibold border-b-2 border-blue-700 pb-1':
								route.fullPath === '/#gallery',
						}"
						>Galeri</a
					>
				</li>
				<li>
					<a
						href="#about"
						class="nav-link hover:text-blue-700 transition"
						:class="{
							'text-blue-700 font-semibold border-b-2 border-blue-700 pb-1': route.fullPath === '/#about',
						}"
						>Tentang</a
					>
				</li>
				<li>
					<a
						href="#contact"
						class="nav-link hover:text-blue-700 transition"
						:class="{
							'text-blue-700 font-semibold border-b-2 border-blue-700 pb-1':
								route.fullPath === '/#contact',
						}"
						>Kontak</a
					>
				</li>
				<li v-if="devUnlocked">
					<NuxtLink
						to="/invoice"
						class="nav-link"
						:class="{
							'text-blue-700 font-semibold border-b-2 border-blue-700 pb-1': route.path === '/invoice',
						}">
						Invoice
					</NuxtLink>
				</li>
				<li v-if="devUnlocked">
					<NuxtLink
						to="/calculator"
						class="nav-link"
						:class="{
							'text-blue-700 font-semibold border-b-2 border-blue-700 pb-1': route.path === '/calculator',
						}">
						Calculator</NuxtLink
					>
				</li>
			</ul>

			<!-- Mobile Menu Icon -->
			<button
				class="md:hidden text-gray-700 hover:text-blue-700 transition"
				@click="mobileMenu = !mobileMenu">
				<svg
					v-if="!mobileMenu"
					xmlns="http://www.w3.org/2000/svg"
					fill="none"
					viewBox="0 0 24 24"
					stroke-width="2"
					stroke="currentColor"
					class="w-7 h-7">
					<path
						stroke-linecap="round"
						stroke-linejoin="round"
						d="M4 6h16M4 12h16M4 18h16" />
				</svg>
				<svg
					v-else
					xmlns="http://www.w3.org/2000/svg"
					fill="none"
					viewBox="0 0 24 24"
					stroke-width="2"
					stroke="currentColor"
					class="w-7 h-7">
					<path
						stroke-linecap="round"
						stroke-linejoin="round"
						d="M6 18L18 6M6 6l12 12" />
				</svg>
			</button>
		</div>

		<!-- Mobile Menu -->
		<transition name="fade">
			<div
				v-if="mobileMenu"
				class="md:hidden bg-white/95 backdrop-blur-lg border-t shadow-inner py-4">
				<ul class="flex flex-col items-center gap-4 text-gray-700 font-medium">
					<li>
						<NuxtLink
							to="/"
							class="hover:text-blue-700 transition"
							@click="mobileMenu = false"
							>Home
						</NuxtLink>
					</li>
					<li v-if="devUnlocked">
						<NuxtLink
							to="/invoice"
							class="nav-link"
							:class="{
								'text-blue-700 font-semibold border-b-2 border-blue-700 pb-1':
									route.path === '/invoice',
							}">
							Invoice
						</NuxtLink>
					</li>
					<li v-if="devUnlocked">
						<NuxtLink
							to="/calculator"
							class="hover:text-blue-700 transition"
							@click="mobileMenu = false">
							Calculator</NuxtLink
						>
					</li>
				</ul>
			</div>
		</transition>
	</nav>
</template>
<script setup>
	useHead({
		title: "Ruslie Spring",
	})
	const route = useRoute()
	const mobileMenu = ref(false)
	const devUnlocked = ref(false)
	const router = useRouter()
	onMounted(() => {
		// restore from storage
		// devUnlocked.value = localStorage.getItem("devUnlocked") === "true"

		// unlock via URL
		if (route.query.dev === "qwerty123") {
			devUnlocked.value = true
			localStorage.setItem("devUnlocked", "true")

			// OPTIONAL: clean URL (?dev=true removed)
			router.replace({ query: {} })
		}
	})
</script>
