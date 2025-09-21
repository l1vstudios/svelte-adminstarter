<script>
  import { onMount } from "svelte";
  import Chart from "chart.js/auto";
  import Icon from "@iconify/svelte";

  // svelte-ignore export_let_unused
  export let name = "Dashboard";

  let dailyIncome = 1200000;
  let weeklyIncome = 8000000;
  let monthlyIncome = 32000000;
  let pendingPayment = 5000000;
  let failedTransactions = 3;

  /**
   * @type {import("chart.js").ChartItem}
   */
  let chartCanvas;

  onMount(() => {
    new Chart(chartCanvas, {
      type: "line",
      data: {
        labels: ["Sen", "Sel", "Rab", "Kam", "Jum", "Sab", "Min"],
        datasets: [
          {
            label: "Pendapatan Harian",
            data: [
              1200000, 1500000, 1000000, 1800000, 2000000, 1700000, 2200000,
            ],
            borderColor: "rgb(59,130,246)",
            backgroundColor: "rgba(59,130,246,0.3)",
            fill: true,
            tension: 0.4,
          },
        ],
      },
      options: {
        responsive: true,
        plugins: {
          legend: { position: "top" },
        },
      },
    });
  });
</script>

<div class="p-6">
  <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
    <div
      class="rounded-xl p-4 shadow-lg text-white bg-gradient-to-r from-blue-500 to-blue-700 flex items-center gap-4"
    >
      <div class="p-3 bg-white/20 rounded-lg">
        <Icon icon="mdi:cash" class="w-8 h-8" />
      </div>
      <div>
        <h2 class="text-sm">Pendapatan Harian</h2>
        <p class="text-xl font-bold">Rp {dailyIncome.toLocaleString()}</p>
      </div>
    </div>

    <div
      class="rounded-xl p-4 shadow-lg text-white bg-gradient-to-r from-green-500 to-green-700 flex items-center gap-4"
    >
      <div class="p-3 bg-white/20 rounded-lg">
        <Icon icon="mdi:calendar-week" class="w-8 h-8" />
      </div>
      <div>
        <h2 class="text-sm">Pendapatan Mingguan</h2>
        <p class="text-xl font-bold">Rp {weeklyIncome.toLocaleString()}</p>
      </div>
    </div>

    <div
      class="rounded-xl p-4 shadow-lg text-white bg-gradient-to-r from-purple-500 to-purple-700 flex items-center gap-4"
    >
      <div class="p-3 bg-white/20 rounded-lg">
        <Icon icon="mdi:calendar-month" class="w-8 h-8" />
      </div>
      <div>
        <h2 class="text-sm">Pendapatan Bulanan</h2>
        <p class="text-xl font-bold">Rp {monthlyIncome.toLocaleString()}</p>
      </div>
    </div>
  </div>

  <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
    <div
      class="rounded-xl p-4 shadow-lg text-white bg-gradient-to-r from-yellow-500 to-yellow-700 flex items-center gap-4"
    >
      <div class="p-3 bg-white/20 rounded-lg">
        <Icon icon="mdi:timer-sand" class="w-8 h-8" />
      </div>
      <div>
        <h2 class="text-sm">Pembayaran Tertahan</h2>
        <p class="text-xl font-bold">Rp {pendingPayment.toLocaleString()}</p>
      </div>
    </div>

    <div
      class="rounded-xl p-4 shadow-lg text-white bg-gradient-to-r from-red-500 to-red-700 flex items-center gap-4"
    >
      <div class="p-3 bg-white/20 rounded-lg">
        <Icon icon="mdi:close-circle" class="w-8 h-8" />
      </div>
      <div>
        <h2 class="text-sm">Transaksi Gagal</h2>
        <p class="text-xl font-bold">{failedTransactions}</p>
      </div>
    </div>
  </div>

  <div class="bg-white shadow-lg rounded-xl p-6">
    <h2 class="text-lg font-semibold mb-4">Grafik Pendapatan Mingguan</h2>
    <canvas bind:this={chartCanvas}></canvas>
  </div>
</div>
