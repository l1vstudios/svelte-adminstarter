<script>
  import Icon from "@iconify/svelte";

  let step = 1;

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

  $: subtotal = (selectedLayanan?.hargaPerKg || 0) * berat;
  $: total = subtotal + biayaAdmin;

  function resetOrder() {
    step = 1;
    selectedLayanan = layananList[0];
    alamat = "";
    whatsapp = "";
    berat = 1;
    metodePembayaran = "";
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
        <span class="hidden sm:inline font-medium">Struk</span>
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
          <label class="block font-medium mb-2">Alamat Penjemputan</label>
          <textarea
            bind:value={alamat}
            rows="3"
            class="border rounded-lg px-3 py-2 w-full shadow-sm focus:ring-2 focus:ring-blue-900 text-sm sm:text-base"
            placeholder="Masukkan alamat penjemputan laundry..."
          ></textarea>
        </div>
        <div class="mb-4">
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
        <div class="grid grid-cols-2 gap-3 sm:gap-4">
          <div
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
          </div>
          <div
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
          </div>
        </div>
      {/if}

      {#if step === 4}
        <h2 class="text-lg sm:text-xl font-bold mb-4 text-blue-900">
          Struk Pesanan
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
          class="mt-4 sm:mt-6 w-full bg-gradient-to-r from-blue-900 to-blue-950 text-white py-2 rounded-lg hover:opacity-90 transition text-sm sm:text-base"
          on:click={resetOrder}
        >
          Pesan Lagi
        </button>
      {/if}
    </div>

    <!-- RINGKASAN -->
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
          class="mt-4 sm:mt-6 w-full bg-gradient-to-r from-blue-900 to-blue-950 text-white py-2 rounded-lg hover:opacity-90 transition text-sm sm:text-base"
          on:click={() => (step = step + 1)}
          disabled={step === 3 && !metodePembayaran}
        >
          {step < 3 ? "Lanjut" : "Bayar"}
        </button>
      {/if}
    </div>
  </div>
</div>

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
</style>
