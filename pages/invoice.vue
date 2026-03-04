<template>
	<div class="min-h-screen bg-gray-50 flex flex-col">
		<!-- Navbar -->
		<Navbar class="no-print" />
		<div
			class="invoice-container mt-24 print:mt-0 bg-white text-gray-800 p-10 max-w-3xl mx-auto shadow-xl rounded-xl relative">
			<!-- Watermark -->
			<div class="absolute inset-0 flex items-center justify-center pointer-events-none select-none">
				<img
					src="/Logo_Ruslie_Spring.png"
					alt="Ruslie Spring Logo"
					class="opacity-20 w-[60%] object-contain" />
			</div>

			<!-- Header -->
			<div class="flex justify-between items-start mb-10 relative z-10">
				<div>
					<h1 class="text-4xl font-extrabold text-blue-900 tracking-tight">INVOICE</h1>
					<p class="mt-2 text-sm leading-5">
						<span class="font-semibold">Ruslie Spring</span><br />
						Jl. Sikatan 45, Tandes, Surabaya<br />
						+62 851 0481 5151
					</p>
				</div>
				<div class="text-right text-sm leading-6">
					<p><strong>Date:</strong> {{ invoice.date }}</p>
					<p><strong>Invoice No:</strong> {{ invoice.number }}</p>
				</div>
			</div>

			<!-- Customer -->
			<div
				class="mb-8 border border-blue-200 rounded-lg p-4 relative z-10 bill-to-editable"
				style="background-color: rgba(191, 219, 254, 0.6)">
				<p class="font-semibold text-blue-900 mb-1">Bill To:</p>
				<div class="grid grid-cols-1 sm:grid-cols-2 gap-2 text-sm">
					<input
						v-model="invoice.customer.name"
						placeholder="Customer Name"
						class="border rounded p-1 text-sm col-span-1 sm:col-span-2" />
					<input
						v-model="invoice.customer.address"
						placeholder="Address Line 1"
						class="border rounded p-1 text-sm col-span-1 sm:col-span-2" />
					<input
						v-model="invoice.customer.city"
						placeholder="City, Zip Code"
						class="border rounded p-1 text-sm" />
					<div class="overflow-hidden flex items-center">
						<span class="mr-1">No Telp :</span>
						<input
							v-model="invoice.customer.phone"
							type="tel"
							placeholder="Phone Number"
							class="border rounded p-1 text-sm flex-grow" />
					</div>
				</div>
				<!-- Payment Method -->
				<div class="payment-method-section text-sm relative mt-4 z-10">
					<p class="font-semibold text-blue-900 mb-2">Payment Method</p>

					<div class="flex items-center gap-4 payment-method">
						<label class="flex items- gap-1">
							<input
								type="radio"
								value="cash"
								v-model="invoice.paymentMethod"
								class="payment-radio" />
							Cash
						</label>

						<label class="flex items-center gap-1">
							<input
								type="radio"
								value="top"
								v-model="invoice.paymentMethod"
								class="payment-radio" />
							TOP
						</label>
					</div>

					<!-- MUNCUL HANYA JIKA TOP -->
					<div v-if="invoice.paymentMethod === 'top'">
						<input
							v-if="invoice.paymentMethod === 'top'"
							v-model="invoice.topNote"
							placeholder="Contoh: TOP 30 Hari"
							class="border rounded p-2 text-sm w-full sm:w-64 payment-input" />
					</div>
				</div>
			</div>

			<!-- Add Item Form -->
			<div class="mb-6 border border-blue-100 rounded-lg p-4 bg-gray-50 no-print relative z-10">
				<h2 class="font-semibold mb-2 text-blue-900">Add Item</h2>
				<div class="grid grid-cols-2 sm:grid-cols-4 gap-2">
					<input
						v-model="newItem.name"
						placeholder="Item name"
						class="col-span-2 border rounded p-2 text-sm" />
					<input
						v-model.number="newItem.qty"
						type="number"
						placeholder="Qty"
						class="border rounded p-2 text-sm" />
					<input
						v-model.number="newItem.price"
						type="number"
						placeholder="Price"
						class="border rounded p-2 text-sm" />
				</div>
				<button
					@click="addItem"
					class="mt-3 px-4 py-1.5 bg-blue-900 text-white rounded text-sm hover:bg-blue-800 transition-all duration-200">
					+ Add
				</button>
			</div>

			<!-- Item Table -->
			<div class="relative z-10">
				<table class="w-full border-collapse border border-gray-300 mb-6 text-sm">
					<thead class="bg-blue-900 text-white">
						<tr>
							<th class="border border-blue-800 p-2 text-left">#</th>
							<th class="border border-blue-800 p-2 text-left">Item</th>
							<th class="border border-blue-800 p-2 text-right">Qty</th>
							<th class="border border-blue-800 p-2 text-right">Unit Price</th>
							<th class="border border-blue-800 p-2 text-right">Total</th>
							<th class="border border-blue-800 p-2 text-center w-16 no-print">Action</th>
						</tr>
					</thead>
					<tbody>
						<tr
							v-for="(item, i) in invoice.items"
							:key="i"
							class="hover:bg-blue-50">
							<td class="border border-gray-300 p-2">{{ i + 1 }}</td>
							<td class="border border-gray-300 p-2">{{ item.name }}</td>
							<td class="border border-gray-300 p-2 text-right">{{ item.qty }}</td>
							<td class="border border-gray-300 p-2 text-right">Rp{{ formatCurrency(item.price) }}</td>
							<td class="border border-gray-300 p-2 text-right">
								Rp{{ formatCurrency(item.qty * item.price) }}
							</td>
							<td class="border border-gray-300 p-2 text-center no-print">
								<button
									@click="removeItem(i)"
									class="text-red-600 hover:underline text-xs">
									Hapus
								</button>
							</td>
						</tr>
					</tbody>
				</table>
			</div>

			<!-- Summary -->
			<div class="text-right relative z-10">
				<p><strong>Subtotal:</strong> Rp{{ formatCurrency(subtotal) }}</p>
				<div class="flex justify-end items-center gap-2 mt-2 text-sm">
					<label
						for="shipping"
						class="text-gray-700"
						>Shipping:</label
					>
					<input
						id="shipping"
						v-model.number="invoice.shipping"
						type="number"
						class="border rounded p-1 text-sm w-24 text-right" />
				</div>
				<p class="text-xl font-bold mt-3 border-t pt-3 text-blue-900">Total: Rp{{ formatCurrency(total) }}</p>
			</div>

			<!-- Footer -->
			<div class="text-center text-xs text-gray-500 mt-10 relative z-10">
				<p>Thank you for your business!</p>
				<p>Payment via BCA - <strong>8620134075</strong> - Albertus Marco Penolla Ruslie</p>
			</div>

			<!-- Print Button -->
			<div class="no-print mt-8 text-center relative z-10">
				<button
					@click="printInvoice"
					class="px-5 py-2 bg-blue-900 text-white rounded-md font-semibold hover:bg-blue-800 shadow transition-all duration-200">
					Print / Save as PDF
				</button>
			</div>
		</div>
	</div>
</template>

<script setup>
	const invoice = reactive({
		date: new Date().toLocaleDateString(),
		number: "INV-",
		customer: {
			name: "Bengkel Berlian Oto Service (Bpk. Jimmy)",
			address: "A. Yani KM.23,7 Jalan Kurnia",
			city: "Landasan Ulin Utara, Banjarbaru 70724",
			phone: "0811512252",
		},
		items: [],
		shipping: 0,
		paymentMethod: "cash", // cash | top
		topNote: "", // misal: TOP 30 hari
	})
	onMounted(() => {
		invoice.number += getFormattedInvoiceNumber()
	})
	const newItem = reactive({ name: "", qty: 0, price: 0 })

	const addItem = () => {
		if (newItem.name && newItem.qty && newItem.price) {
			invoice.items.push({ ...newItem })
			Object.assign(newItem, { name: "", qty: 0, price: 0 })
		}
	}
	const getFormattedInvoiceNumber = () => {
		const date = new Date()

		// Format date as YYMMDD (e.g. 25 10 04 for 2025 Oct 04)
		const yy = String(date.getFullYear()).slice(-2)
		const mm = String(date.getMonth() + 1).padStart(2, "0")
		const dd = String(date.getDate()).padStart(2, "0")
		const datePart = yy + mm + dd

		// Generate two random uppercase letters (A–Z)
		const letters = Array.from({ length: 2 }, () => String.fromCharCode(65 + Math.floor(Math.random() * 26))).join(
			"",
		)

		return `${datePart}-${letters}`
	}
	const removeItem = (i) => invoice.items.splice(i, 1)

	const formatCurrency = (val) => val.toLocaleString("id-ID", { minimumFractionDigits: 0 })

	const subtotal = computed(() => invoice.items.reduce((acc, item) => acc + item.qty * item.price, 0))

	const total = computed(() => subtotal.value + (invoice.shipping || 0))

	const printInvoice = () => window.print()
</script>

<style>
	/* Hide non-printable elements in PDF/print */
	@media print {
		.no-print {
			display: none !important;
		}

		body {
			margin: 0;
			padding: 0;
			height: 100vh;
			-webkit-print-color-adjust: exact;
		}

		/* NEW: Styles for inputs inside the editable Bill To section */
		.bill-to-editable input {
			border: none;
			/* Remove the border */
			background-color: transparent;
			/* Make the background transparent */
			padding: 0;
			margin: 0;
			/* Adjust padding for cleaner look */
		}
		.payment-radio {
			display: none !important;
		}

		/* Hide label yang radio-nya TIDAK dipilih */
		label:has(input.payment-radio:not(:checked)) {
			display: none !important;
		}

		/* Input jadi teks (tanpa border) */
		.payment-input {
			border: none !important;
			background: transparent;
			padding: 0 !important;
		}

		/* Jaga layout tetap rapi */
		.payment-method {
			gap: 0 !important;
		}
	}
</style>
