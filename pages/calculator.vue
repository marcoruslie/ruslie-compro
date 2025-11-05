<template>
	<div class="min-h-screen bg-gray-50 flex flex-col">
		<!-- Navbar -->
		<Navbar />


		<main class="pt-24 px-6 pb-16 max-w-4xl mx-auto">
			<h2 class="text-3xl font-bold text-center mb-6 animate-slide-up" :style="{ color: primaryColor }">
				Spring & Wire Price Calculator
			</h2>
			<p class="text-center text-gray-600 mb-10 animate-fade-in">
				Masukkan data spesifikasi pegas untuk menghitung estimasi berat dan harga bahan.
			</p>

			<!-- Calculator Card -->
			<div class="bg-white shadow-xl rounded-2xl p-8 animate-fade-in">
				<form @submit.prevent="calculate">
					<div class="grid grid-cols-1 md:grid-cols-2 gap-6">
						<!-- Field -->
						<Field label="Diameter Kawat (mm) *" icon="ruler-combined" v-model="form.wireDiameter"
							step="0.01" placeholder="Contoh: 0.8" />
						<Field label="Diameter Dalam (mm) *" icon="circle-notch" v-model="form.innerDiameter"
							step="0.01" placeholder="Contoh: 14" />
						<Field label="Panjang (mm) *" icon="ruler-vertical" v-model="form.length" step="0.1"
							placeholder="Contoh: 50" />
						<Field label="Konstanta *" icon="gauge" v-model="form.constant" step="0.1"
							placeholder="Nilai konstanta" />
						<Field label="Jarak Lilitan / Pitch (mm)" icon="wave-square" v-model="form.pitch" step="0.1"
							placeholder="Contoh: 3.5" />
						<Field label="Jumlah Lilitan" icon="rotate" v-model="form.coils" placeholder="Jumlah lilitan" />
						<!-- NOTE -->
						<div class="text-sm text-gray-500 italic mt-6 md:mt-0 md:col-span-2">
							* Jika jumlah lilitan tidak diisi atau diisi 0, maka akan dihitung otomatis berdasarkan
							panjang dan pitch (jika diisi). Jika pitch juga tidak diisi, maka akan diasumsikan pitch =
							diameter kawat.
						</div>
					</div>

					<div class="mt-8 flex justify-center">
						<button type="submit"
							class="bg-[#033272] hover:bg-blue-800 text-white font-semibold px-6 py-2 rounded-lg shadow transition">
							Hitung
						</button>
					</div>
				</form>

				<!-- Results -->
				<div v-if="result" class="mt-10 border-t pt-6 animate-slide-up" ref="resultSection">
					<h3 class="text-2xl font-bold mb-4" :style="{ color: primaryColor }">Hasil Perhitungan</h3>
					<ul class="space-y-2 text-gray-700">
						<li><strong>Berat Pegas:</strong> {{ result.weight.toFixed(2) }} gram</li>
						<li><strong>Harga Baja:</strong> Rp {{ result.priceSteel.toLocaleString() }}</li>
						<li><strong>Harga Stainless:</strong> Rp {{ result.priceStainless.toLocaleString() }}</li>
					</ul>
				</div>
			</div>
		</main>

		<!-- Footer -->
		<footer class="bg-[#021d47] text-white mt-16 py-8 text-center text-sm">
			© {{ new Date().getFullYear() }} Ruslie Spring. All rights reserved.
		</footer>

		<!-- WhatsApp -->
		<a href="https://wa.me/6285104815151" target="_blank"
			class="fixed bottom-6 right-6 bg-green-500 hover:bg-green-600 text-white p-4 rounded-full shadow-lg">
			💬
		</a>
	</div>
</template>

<script setup>
import Field from '~/components/Field.vue'


const primaryColor = '#033272'
const resultSection = ref(null)
// form data
const form = reactive({
	wireDiameter: null,
	innerDiameter: null,
	length: null,
	constant: null,
	pitch: null,
	coils: null
})

const result = ref(null)

async function calculate() {
	const { wireDiameter, innerDiameter, length, constant } = form

	// approximate length of wire in mm (simplified helix)
	const outerDiameter = innerDiameter + wireDiameter * 2

	// a. Berat Kawat per Meter
	const volume = Math.PI * (wireDiameter / 2) ** 2 * 7.89 // mm³

	// b. Panjang Kawat per Lilitan
	//    dibulatkan keatas
	const lengthPerCoil = Math.ceil(Math.PI * outerDiameter)

	// c. Jumlah Lilitan
	if (form.coils === 0 || !form.coils) {
		if (!form.pitch || form.pitch === 0) {
			form.coils = length / (2 * wireDiameter) // asumsi pitch = 8 x diameter kawat
			form.pitch = wireDiameter
			form.coils = Math.ceil(form.coils)
		}
		else if (form.pitch && form.pitch > 0) {
			form.coils = length / form.pitch
			//DIBULATKAN KE BAWA
			form.coils = Math.ceil(form.coils)
		}
	}
	// Rumus Utama Perhitungan
	const totalWeight = (form.coils * lengthPerCoil) / 1000 * volume
	const pricePerKgSteel = 90
	const pricePerKgStainless = 180
	if (totalWeight) {
		result.value = {
			weight: totalWeight,
			priceSteel: Math.ceil(totalWeight * pricePerKgSteel * constant),
			priceStainless: Math.ceil(totalWeight * pricePerKgStainless * constant)
		}
	}
	await nextTick()
	if (resultSection.value) {
		// Offset in pixels (adjust as needed)
		const yOffset = 100 // scroll 100px further down
		const y = resultSection.value.getBoundingClientRect().top + window.scrollY + yOffset
		resultSection.value.scrollIntoView({ behavior: 'smooth', top: y })
	}
}
</script>

<style scoped>
.nav-link {
	position: relative;
	transition: color 0.3s ease;
}

.nav-link::after {
	content: '';
	position: absolute;
	left: 0;
	bottom: -3px;
	width: 0%;
	height: 2px;
	background-color: #2563eb;
	transition: width 0.3s ease;
}

.nav-link:hover::after {
	width: 100%;
}

.fade-enter-active,
.fade-leave-active {
	transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
	opacity: 0;
}

.animate-fade-in {
	animation: fade-in 1s ease forwards;
}

.animate-slide-up {
	animation: slide-up 0.8s ease forwards;
}

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
		transform: translateY(20px);
	}

	to {
		opacity: 1;
		transform: translateY(0);
	}
}
</style>
