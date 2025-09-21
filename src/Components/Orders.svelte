<script>
  import { onMount } from "svelte";

  let allTransactions = [
    {
      id: 1,
      nama: "Budi Santoso",
      total: 25000,
      totalKg: 3.5,
      status: "Lunas",
    },
    {
      id: 2,
      nama: "Siti Nurhaliza",
      total: 45000,
      totalKg: 6.0,
      status: "Pending",
    },
    {
      id: 3,
      nama: "Ahmad Rahman",
      total: 30000,
      totalKg: 4.2,
      status: "Lunas",
    },
    {
      id: 4,
      nama: "Dewi Sartika",
      total: 20000,
      totalKg: 2.8,
      status: "Belum Bayar",
    },
    {
      id: 5,
      nama: "Rizki Pratama",
      total: 55000,
      totalKg: 7.5,
      status: "Lunas",
    },
    {
      id: 6,
      nama: "Maya Indah",
      total: 35000,
      totalKg: 4.8,
      status: "Pending",
    },
    {
      id: 7,
      nama: "Doni Setiawan",
      total: 40000,
      totalKg: 5.5,
      status: "Lunas",
    },
    {
      id: 8,
      nama: "Rina Anggraeni",
      total: 25000,
      totalKg: 3.2,
      status: "Belum Bayar",
    },
    {
      id: 9,
      nama: "Fajar Nugroho",
      total: 50000,
      totalKg: 6.8,
      status: "Lunas",
    },
    {
      id: 10,
      nama: "Lina Marlina",
      total: 30000,
      totalKg: 4.0,
      status: "Pending",
    },
    { id: 11, nama: "Eko Wijaya", total: 60000, totalKg: 8.2, status: "Lunas" },
    {
      id: 12,
      nama: "Indira Sari",
      total: 28000,
      totalKg: 3.8,
      status: "Belum Bayar",
    },
  ];

  let currentPage = 1;
  const itemsPerPage = 5;

  $: totalPages = Math.ceil(allTransactions.length / itemsPerPage);
  $: startIndex = (currentPage - 1) * itemsPerPage;
  $: endIndex = startIndex + itemsPerPage;
  $: paginatedTransactions = allTransactions.slice(startIndex, endIndex);

  // @ts-ignore
  function formatCurrency(amount) {
    return new Intl.NumberFormat("id-ID", {
      style: "currency",
      currency: "IDR",
      minimumFractionDigits: 0,
    }).format(amount);
  }

  /**
   * @param {string} status
   */
  function getStatusClass(status) {
    switch (status) {
      case "Lunas":
        return "bg-green-100 text-green-800 border-green-200";
      case "Pending":
        return "bg-yellow-100 text-yellow-800 border-yellow-200";
      case "Belum Bayar":
        return "bg-red-100 text-red-800 border-red-200";
      default:
        return "bg-gray-100 text-gray-800 border-gray-200";
    }
  }

  /**
   * @param {string} nama
   */
  function getInitials(nama) {
    return nama
      .split(" ")
      .map((n) => n[0])
      .join("")
      .substring(0, 2);
  }

  /**
   * @param {number} page
   */
  function goToPage(page) {
    if (page >= 1 && page <= totalPages) {
      currentPage = page;
    }
  }

  function previousPage() {
    if (currentPage > 1) {
      currentPage--;
    }
  }

  function nextPage() {
    if (currentPage < totalPages) {
      currentPage++;
    }
  }

  // Generate page numbers for pagination
  $: pageNumbers = Array.from({ length: totalPages }, (_, i) => i + 1);
</script>

<svelte:head>
  <link
    rel="stylesheet"
    href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css"
  />
</svelte:head>

<div class="bg-gray-50 min-h-screen">
  <div class="container mx-auto px-4 py-8">
    <div
      class="bg-blue-50 rounded-xl shadow-lg p-6 mb-8 border-b-4 border-blue-900"
    >
      <div class="flex items-center justify-between">
        <div class="flex items-center space-x-4">
          <div class="bg-blue-100 p-4 rounded-full">
            <i class="fas fa-tshirt text-3xl text-blue-600"></i>
          </div>
          <div>
            <h1 class="text-2xl font-bold text-gray-800">Orderan</h1>
            <!-- <p class="text-gray-600">Sistem Manajemen Transaksi</p> -->
          </div>
        </div>
        <div class="text-right">
          <p class="text-sm text-gray-500">Total Transaksi</p>
          <p class="text-2xl font-bold text-blue-600">
            {allTransactions.length}
          </p>
        </div>
      </div>
    </div>

    <div class="bg-white rounded-xl shadow-lg overflow-hidden">
      <div class="px-6 py-4 border-b border-gray-200">
        <h2 class="text-xl font-semibold text-gray-800 flex items-center">
          <i class="fas fa-list-alt mr-2 text-blue-500"></i>
          Data Transaksi Laundry
        </h2>
      </div>

      <div class="overflow-x-auto">
        <table class="w-full">
          <thead class="bg-gray-50">
            <tr>
              <th
                class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
              >
                No
              </th>
              <th
                class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
              >
                Nama Pelanggan
              </th>
              <th
                class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
              >
                Total Harga
              </th>
              <th
                class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
              >
                Total (Kg)
              </th>
              <th
                class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
              >
                Status Pembayaran
              </th>
            </tr>
          </thead>
          <tbody class="bg-white divide-y divide-gray-200">
            {#each paginatedTransactions as transaction, index}
              <tr class="hover:bg-gray-50 transition-colors duration-200">
                <td
                  class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900"
                >
                  {startIndex + index + 1}
                </td>
                <td class="px-6 py-4 whitespace-nowrap">
                  <div class="flex items-center">
                    <div class="flex-shrink-0 h-8 w-8">
                      <div
                        class="h-8 w-8 rounded-full bg-gradient-to-r from-blue-400 to-blue-600 flex items-center justify-center"
                      >
                        <span class="text-sm font-medium text-white">
                          {getInitials(transaction.nama)}
                        </span>
                      </div>
                    </div>
                    <div class="ml-3">
                      <div class="text-sm font-medium text-gray-900">
                        {transaction.nama}
                      </div>
                    </div>
                  </div>
                </td>
                <td
                  class="px-6 py-4 whitespace-nowrap text-sm font-semibold text-green-600"
                >
                  {formatCurrency(transaction.total)}
                </td>
                <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-900">
                  <span
                    class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium bg-blue-100 text-blue-800"
                  >
                    <i class="fas fa-weight-hanging mr-1"></i>
                    {transaction.totalKg} kg
                  </span>
                </td>
                <td class="px-6 py-4 whitespace-nowrap">
                  <span
                    class="inline-flex items-center px-3 py-1 rounded-full text-xs font-medium border {getStatusClass(
                      transaction.status,
                    )}"
                  >
                    <i class="fas fa-circle mr-1 text-xs"></i>
                    {transaction.status}
                  </span>
                </td>
              </tr>
            {/each}
          </tbody>
        </table>
      </div>

      <div class="bg-gray-50 px-6 py-3 border-t border-gray-200">
        <div class="flex items-center justify-between">
          <div class="flex items-center text-sm text-gray-700">
            <span>
              Menampilkan
              <span class="font-medium">{startIndex + 1}</span>
              sampai
              <span class="font-medium"
                >{Math.min(endIndex, allTransactions.length)}</span
              >
              dari
              <span class="font-medium">{allTransactions.length}</span>
              hasil
            </span>
          </div>
          <div class="flex items-center space-x-2">
            <button
              on:click={previousPage}
              disabled={currentPage === 1}
              class="relative inline-flex items-center px-3 py-2 text-sm font-medium rounded-md text-gray-500 bg-white border border-gray-300 hover:bg-gray-50 hover:text-gray-700 disabled:opacity-50 disabled:cursor-not-allowed transition-colors duration-200"
            >
              <i class="fas fa-chevron-left mr-1"></i>
              Previous
            </button>

            <div class="flex space-x-1">
              {#each pageNumbers as page}
                <button
                  on:click={() => goToPage(page)}
                  class="relative inline-flex items-center px-3 py-2 text-sm font-medium rounded-md transition-colors duration-200 {page ===
                  currentPage
                    ? 'bg-blue-600 text-white border border-blue-600'
                    : 'text-gray-500 bg-white border border-gray-300 hover:bg-gray-50 hover:text-gray-700'}"
                >
                  {page}
                </button>
              {/each}
            </div>

            <button
              on:click={nextPage}
              disabled={currentPage === totalPages}
              class="relative inline-flex items-center px-3 py-2 text-sm font-medium rounded-md text-gray-500 bg-white border border-gray-300 hover:bg-gray-50 hover:text-gray-700 disabled:opacity-50 disabled:cursor-not-allowed transition-colors duration-200"
            >
              Next
              <i class="fas fa-chevron-right ml-1"></i>
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
