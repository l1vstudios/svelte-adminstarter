<script>
  import Icon from "@iconify/svelte";

  let step = 1;
  let showReceiptModal = false;

  let layananList = [
    {
      id: 1,
      nama: "Cuci Kering",
      hargaPerKg: 8000,
      icon: "ph:washing-machine",
    },
    { id: 2, nama: "Cuci Setrika", hargaPerKg: 12000, icon: "mdi:iron" },
    {
      id: 3,
      nama: "Laundry Express",
      hargaPerKg: 18000,
      icon: "mdi:truck-delivery",
    },
    {
      id: 4,
      nama: "Laundry Premium",
      hargaPerKg: 25000,
      icon: "mdi:tshirt-crew",
    },
  ];

  let selectedLayanan = layananList[0];
  let alamat = "";
  let whatsapp = "";
  let berat = 1;
  let biayaAdmin = 2000;
  let metodePembayaran = "";
  let orderNumber = "";
  let orderDate = "";

  $: subtotal = (selectedLayanan?.hargaPerKg || 0) * berat;
  $: total = subtotal + biayaAdmin;

  // Generate QR Code URL with payment info
  $: qrCodeUrl = `https://api.qrserver.com/v1/create-qr-code/?size=200x200&data=QRIS_PAYMENT_${total}_${orderNumber || "PENDING"}`;

  function generateOrderNumber() {
    return "LDR" + Date.now().toString().slice(-8);
  }

  function generateOrderDate() {
    return new Date().toLocaleDateString("id-ID", {
      year: "numeric",
      month: "long",
      day: "numeric",
      hour: "2-digit",
      minute: "2-digit",
    });
  }

  function completeOrder() {
    orderNumber = generateOrderNumber();
    orderDate = generateOrderDate();
    showReceiptModal = true;
  }

  function printReceipt() {
    // @ts-ignore
    const printContent = document.getElementById("receipt-content").innerHTML;
    const originalContent = document.body.innerHTML;

    // Create print-friendly CSS
    const printStyles = `
      <style>
        @media print {
          body { margin: 0; padding: 20px; font-family: 'Courier New', monospace; }
          .receipt { width: 300px; margin: 0 auto; }
          .no-print { display: none !important; }
        }
      </style>
    `;

    document.body.innerHTML =
      printStyles + '<div class="receipt">' + printContent + "</div>";
    window.print();
    document.body.innerHTML = originalContent;

    // Re-bind events after content restoration
    location.reload();
  }

  function resetOrder() {
    step = 1;
    selectedLayanan = layananList[0];
    alamat = "";
    whatsapp = "";
    berat = 1;
    metodePembayaran = "";
    showReceiptModal = false;
    orderNumber = "";
    orderDate = "";
  }
</script>

<div class="p-4 sm:p-6 max-w-6xl mx-auto">
  <!-- STEP INDICATOR -->
  <div class="bg-white shadow-lg rounded-xl p-3 sm:p-6 mb-6">
    <div
      class="flex items-center justify-between sm:justify-center sm:space-x-6 text-xs sm:text-base"
    >
      <!-- Step 1 -->
      <div class="flex items-center space-x-1 sm:space-x-2">
        <div
          class="w-8 h-8 sm:w-12 sm:h-12 flex items-center justify-center rounded-full text-white text-lg sm:text-2xl bg-gradient-to-r from-blue-900 to-blue-950"
          class:heartbeat={step === 1}
        >
          <Icon icon="ph:washing-machine" />
        </div>
        <span class="hidden sm:inline font-medium">Pilih Layanan</span>
      </div>
      <div class="hidden sm:block w-16 h-0.5 bg-gray-300"></div>

      <!-- Step 2 -->
      <div class="flex items-center space-x-1 sm:space-x-2">
        <div
          class="w-8 h-8 sm:w-12 sm:h-12 flex items-center justify-center rounded-full text-white text-lg sm:text-2xl bg-gradient-to-r from-blue-900 to-blue-950"
          class:opacity-30={step < 2}
          class:heartbeat={step === 2}
        >
          <Icon icon="mdi:map-marker" />
        </div>
        <span class="hidden sm:inline font-medium">Isi Detail</span>
      </div>
      <div class="hidden sm:block w-16 h-0.5 bg-gray-300"></div>

      <!-- Step 3 -->
      <div class="flex items-center space-x-1 sm:space-x-2">
        <div
          class="w-8 h-8 sm:w-12 sm:h-12 flex items-center justify-center rounded-full text-white text-lg sm:text-2xl bg-gradient-to-r from-blue-900 to-blue-950"
          class:opacity-30={step < 3}
          class:heartbeat={step === 3}
        >
          <Icon icon="mdi:cash-multiple" />
        </div>
        <span class="hidden sm:inline font-medium">Pembayaran</span>
      </div>
      <div class="hidden sm:block w-16 h-0.5 bg-gray-300"></div>

      <!-- Step 4 -->
      <div class="flex items-center space-x-1 sm:space-x-2">
        <div
          class="w-8 h-8 sm:w-12 sm:h-12 flex items-center justify-center rounded-full text-white text-lg sm:text-2xl bg-gradient-to-r from-blue-900 to-blue-950"
          class:opacity-30={step < 4}
          class:heartbeat={step === 4}
        >
          <Icon icon="mdi:check-bold" />
        </div>
        <span class="hidden sm:inline font-medium">Konfirmasi</span>
      </div>
    </div>
  </div>

  <!-- CONTENT -->
  <div class="grid grid-cols-1 lg:grid-cols-3 gap-4 sm:gap-6">
    <!-- FORM & STEPS -->
    <div class="lg:col-span-2 bg-white rounded-2xl shadow-lg p-4 sm:p-6">
      {#if step === 1}
        <h2 class="text-lg sm:text-xl font-bold mb-4 text-blue-900">
          Pilih Layanan Laundry
        </h2>
        <div class="grid grid-cols-2 gap-3 sm:gap-4 mb-6">
          {#each layananList as l}
            <!-- svelte-ignore a11y_click_events_have_key_events -->
            <!-- svelte-ignore a11y_no_static_element_interactions -->
            <div
              class="border rounded-2xl p-3 sm:p-5 shadow hover:shadow-lg cursor-pointer transition bg-white flex flex-col items-center text-center"
              class:border-blue-900={selectedLayanan?.id === l.id}
              class:ring-2={selectedLayanan?.id === l.id}
              class:ring-blue-900={selectedLayanan?.id === l.id}
              on:click={() => (selectedLayanan = l)}
            >
              <Icon
                icon={l.icon}
                class="text-3xl sm:text-4xl text-blue-900 mb-2"
              />
              <p class="font-semibold text-gray-800 text-sm sm:text-base">
                {l.nama}
              </p>
              <p class="text-blue-900 font-bold text-sm sm:text-base">
                Rp {l.hargaPerKg.toLocaleString("id-ID")}/kg
              </p>
            </div>
          {/each}
        </div>
      {/if}

      {#if step === 2}
        <h2 class="text-lg sm:text-xl font-bold mb-4 text-blue-900">
          Isi Detail Pesanan
        </h2>
        <div class="mb-4">
          <!-- svelte-ignore a11y_label_has_associated_control -->
          <label class="block font-medium mb-2">Alamat Penjemputan</label>
          <textarea
            bind:value={alamat}
            rows="3"
            class="border rounded-lg px-3 py-2 w-full shadow-sm focus:ring-2 focus:ring-blue-900 text-sm sm:text-base"
            placeholder="Masukkan alamat penjemputan laundry..."
          ></textarea>
        </div>
        <div class="mb-4">
          <!-- svelte-ignore a11y_label_has_associated_control -->
          <label class="block font-medium mb-2"
            >Nomor Whatsapp (628123456789)</label
          >
          <input
            type="text"
            bind:value={whatsapp}
            class="border rounded-lg px-3 py-2 w-full shadow-sm focus:ring-2 focus:ring-blue-900 text-sm sm:text-base"
          />
        </div>
        <div class="mb-4">
          <!-- svelte-ignore a11y_label_has_associated_control -->
          <label class="block font-medium mb-2">Berat Laundry (kg)</label>
          <input
            type="number"
            min="1"
            bind:value={berat}
            class="border rounded-lg px-3 py-2 w-full shadow-sm focus:ring-2 focus:ring-blue-900 text-sm sm:text-base"
          />
        </div>
      {/if}

      {#if step === 3}
        <h2 class="text-lg sm:text-xl font-bold mb-4 text-blue-900">
          Pilih Metode Pembayaran
        </h2>
        <div class="grid grid-cols-2 gap-3 sm:gap-4 mb-6">
          <div class="grid grid-cols-2 gap-3 sm:gap-4 mb-6">
            <button
              type="button"
              class="border rounded-xl p-3 sm:p-4 flex items-center justify-center cursor-pointer hover:shadow-lg text-sm sm:text-base"
              class:ring-2={metodePembayaran === "Cash"}
              class:ring-blue-900={metodePembayaran === "Cash"}
              on:click={() => (metodePembayaran = "Cash")}
            >
              <Icon
                icon="mdi:cash"
                class="text-2xl sm:text-3xl text-green-600 mr-2"
              />
              <span>Cash</span>
            </button>

            <button
              type="button"
              class="border rounded-xl p-3 sm:p-4 flex items-center justify-center cursor-pointer hover:shadow-lg text-sm sm:text-base"
              class:ring-2={metodePembayaran === "QRIS"}
              class:ring-blue-900={metodePembayaran === "QRIS"}
              on:click={() => (metodePembayaran = "QRIS")}
            >
              <Icon
                icon="mdi:qrcode-scan"
                class="text-2xl sm:text-3xl text-blue-600 mr-2"
              />
              <span>QRIS</span>
            </button>
          </div>
        </div>

        {#if metodePembayaran === "QRIS"}
          <div
            class="mt-6 bg-gradient-to-br from-blue-50 to-indigo-50 rounded-2xl p-6 shadow-lg border border-blue-200"
          >
            <div class="text-center">
              <h3
                class="text-lg font-bold text-blue-900 mb-4 flex items-center justify-center"
              >
                <Icon icon="mdi:qrcode-scan" class="text-2xl mr-2" />
                Scan QR Code untuk Pembayaran
              </h3>

              <div
                class="bg-white rounded-xl p-4 shadow-md inline-block border-2 border-blue-200"
              >
                <img
                  src={qrCodeUrl}
                  alt="QRIS Payment QR Code"
                  class="w-48 h-48 mx-auto"
                  style="image-rendering: pixelated;"
                />
              </div>

              <div
                class="mt-4 bg-white rounded-lg p-4 shadow-sm border border-blue-100"
              >
                <div class="flex justify-between items-center mb-2">
                  <span class="text-gray-600">Total :</span>
                  <span class="font-bold text-xl text-blue-900">
                    Rp {total.toLocaleString("id-ID")}
                  </span>
                </div>
                <div class="text-sm text-gray-500">
                  Scan QR Code dengan aplikasi e-wallet Anda
                </div>
              </div>

              <div
                class="mt-4 text-left bg-blue-50 rounded-lg p-4 border border-blue-200"
              >
                <h4 class="font-semibold text-blue-900 mb-2 flex items-center">
                  <Icon icon="mdi:information" class="mr-2" />
                  Cara Pembayaran:
                </h4>
                <ol class="text-sm text-blue-800 space-y-1">
                  <li>1. Buka aplikasi e-wallet (GoPay, OVO, Dana, dll)</li>
                  <li>2. Pilih menu "Scan QR" atau "QRIS"</li>
                  <li>3. Arahkan kamera ke QR Code di atas</li>
                  <li>
                    4. Konfirmasi pembayaran sebesar <strong
                      >Rp {total.toLocaleString("id-ID")}</strong
                    >
                  </li>
                  <li>5. Selesaikan pembayaran</li>
                </ol>
              </div>
            </div>
          </div>
        {/if}
      {/if}

      {#if step === 4}
        <h2 class="text-lg sm:text-xl font-bold mb-4 text-blue-900">
          Konfirmasi Pesanan
        </h2>
        <div
          class="border rounded-xl p-4 sm:p-6 bg-gray-50 shadow text-sm sm:text-base"
        >
          <p><strong>Layanan:</strong> {selectedLayanan?.nama}</p>
          <p><strong>Alamat:</strong> {alamat}</p>
          <p><strong>No. Whatsapp:</strong> {whatsapp}</p>
          <p><strong>Berat:</strong> {berat} kg</p>
          <p><strong>Metode Pembayaran:</strong> {metodePembayaran}</p>
          <hr class="my-3" />
          <p class="flex justify-between">
            <span>Biaya Laundry</span><span
              >Rp {subtotal.toLocaleString("id-ID")}</span
            >
          </p>
          <p class="flex justify-between">
            <span>Biaya Admin</span><span
              >Rp {biayaAdmin.toLocaleString("id-ID")}</span
            >
          </p>
          <p class="flex justify-between font-bold text-blue-900">
            <span>Total</span><span>Rp {total.toLocaleString("id-ID")}</span>
          </p>
        </div>
        <button
          class="mt-4 sm:mt-6 w-full bg-gradient-to-r from-blue-900 to-blue-950 text-white py-3 rounded-lg hover:opacity-90 transition text-sm sm:text-base font-semibold"
          on:click={completeOrder}
        >
          <Icon icon="mdi:check-circle" class="inline mr-2" />
          Konfirmasi & Lihat Struk
        </button>
      {/if}
    </div>

    <div class="bg-white rounded-2xl shadow-lg p-4 sm:p-6 text-sm sm:text-base">
      <h2 class="text-lg sm:text-xl font-bold mb-4 text-blue-900">
        Ringkasan Pesanan
      </h2>
      <p><strong>Layanan:</strong> {selectedLayanan?.nama}</p>
      <p><strong>Alamat:</strong> {alamat || "-"}</p>
      <p><strong>No WA:</strong> {whatsapp || "-"}</p>
      <p><strong>Berat:</strong> {berat} kg</p>
      <p><strong>Pembayaran:</strong> {metodePembayaran || "-"}</p>

      <div class="mt-4 border-t pt-4">
        <p class="flex justify-between">
          <span>Biaya Laundry</span><span
            >Rp {subtotal.toLocaleString("id-ID")}</span
          >
        </p>
        <p class="flex justify-between">
          <span>Biaya Admin</span><span
            >Rp {biayaAdmin.toLocaleString("id-ID")}</span
          >
        </p>
        <p class="flex justify-between font-bold text-blue-900">
          <span>Total</span><span>Rp {total.toLocaleString("id-ID")}</span>
        </p>
      </div>

      {#if step < 4}
        <button
          class="mt-4 sm:mt-6 w-full bg-gradient-to-r from-blue-900 to-blue-950 text-white py-2 rounded-lg hover:opacity-90 transition text-sm sm:text-base flex items-center justify-center gap-2"
          on:click={() => (step = step + 1)}
          disabled={step === 3 && !metodePembayaran}
        >
          <span>{step < 3 ? "Lanjut" : "Konfirmasi"}</span>
          <Icon icon="mdi:chevron-right" class="text-lg sm:text-xl" />
        </button>
      {/if}
    </div>
  </div>
</div>

{#if showReceiptModal}
  <div
    class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 p-4"
  >
    <div
      class="bg-white rounded-lg shadow-2xl max-w-md w-full max-h-[90vh] overflow-y-auto"
    >
      <!-- Modal Header -->
      <div class="flex justify-between items-center p-4 border-b no-print">
        <h3 class="text-lg font-semibold text-gray-800">Struk Pesanan</h3>
        <button
          on:click={() => (showReceiptModal = false)}
          class="text-gray-400 hover:text-gray-600"
        >
          <Icon icon="mdi:close" class="text-xl" />
        </button>
      </div>

      <div id="receipt-content" class="p-6">
        <div
          class="text-center border-b-2 border-dashed border-gray-300 pb-4 mb-4"
        >
          <div class="text-xs text-gray-500 mb-2">
            ••••••••••••••••••••••••••••••••••••••••••
          </div>
          <h2 class="font-bold text-lg text-gray-800">RECEIPT</h2>
          <h3 class="font-bold text-base text-gray-700 mt-1">LAUNDRY ONLINE</h3>
          <div class="text-xs text-gray-500 mt-2">
            ••••••••••••••••••••••••••••••••••••••••••
          </div>
        </div>

        <div class="text-sm space-y-1 mb-4">
          <div class="flex justify-between">
            <span>Order No:</span>
            <span class="font-mono">{orderNumber}</span>
          </div>
          <div class="flex justify-between">
            <span>Date:</span>
            <span>{orderDate}</span>
          </div>
          <div class="flex justify-between">
            <span>Customer:</span>
            <span>{whatsapp}</span>
          </div>
        </div>

        <div class="border-t border-dashed border-gray-400 pt-3 mb-4">
          <h4 class="font-semibold mb-2">Description</h4>
          <div class="text-sm space-y-1">
            <div class="flex justify-between">
              <span>{selectedLayanan?.nama}</span>
              <span
                >Rp {selectedLayanan?.hargaPerKg.toLocaleString("id-ID")}</span
              >
            </div>
            <div class="flex justify-between text-gray-600">
              <span>Weight: {berat} kg</span>
              <span>Rp {subtotal.toLocaleString("id-ID")}</span>
            </div>
            <div class="flex justify-between text-gray-600">
              <span>Admin Fee</span>
              <span>Rp {biayaAdmin.toLocaleString("id-ID")}</span>
            </div>
            <div class="flex justify-between text-gray-600">
              <span>Payment</span>
              <span>{metodePembayaran}</span>
            </div>
          </div>
        </div>

        <div class="border-t border-dashed border-gray-400 pt-3 mb-4">
          <div class="flex justify-between text-sm mb-1">
            <span>Subtotal</span>
            <span>Rp {subtotal.toLocaleString("id-ID")}</span>
          </div>
          <div class="flex justify-between text-sm mb-1">
            <span>Admin Fee</span>
            <span>Rp {biayaAdmin.toLocaleString("id-ID")}</span>
          </div>
          <div class="flex justify-between font-bold text-lg border-t pt-2">
            <span>TOTAL</span>
            <span>Rp {total.toLocaleString("id-ID")}</span>
          </div>
        </div>

        <div class="text-center py-4 border-t border-dashed border-gray-400">
          <p class="font-bold text-lg">THANK YOU</p>
          <p class="text-sm text-gray-600 mt-2">Alamat: {alamat}</p>
        </div>

        <div class="text-center mt-4">
          <div class="text-xs text-gray-500 mb-2">
            ••••••••••••••••••••••••••••••••••••••••••
          </div>
          <div class="font-mono text-xs bg-black text-white p-2 inline-block">
            ||||| |||| | ||| |||| | |||| |||||
          </div>
          <div class="font-mono text-xs text-gray-600 mt-1">{orderNumber}</div>
          <div class="text-xs text-gray-500 mt-2 mb-4">
            ••••••••••••••••••••••••••••••••••••••••••
          </div>
        </div>
      </div>

      <div class="flex gap-3 p-4 border-t no-print">
        <button
          on:click={printReceipt}
          class="flex-1 bg-blue-600 text-white py-2 px-4 rounded-lg hover:bg-blue-700 transition flex items-center justify-center"
        >
          <Icon icon="mdi:printer" class="mr-2" />
          Print PDF
        </button>
        <button
          on:click={resetOrder}
          class="flex-1 bg-gradient-to-r from-blue-900 to-blue-950 text-white py-2 rounded-lg hover:opacity-90 transition text-sm sm:text-base flex items-center justify-center"
        >
          <Icon icon="mdi:cart" class="mr-2" />
          Order Baru
        </button>
      </div>
    </div>
  </div>
{/if}

<style>
  @keyframes heartbeat {
    0%,
    40%,
    80%,
    100% {
      transform: scale(1);
    }
    20%,
    60% {
      transform: scale(1.2);
    }
  }
  .heartbeat {
    animation: heartbeat 1s infinite;
  }

  @media print {
    .no-print {
      display: none !important;
    }
  }
</style>
