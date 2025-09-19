<script>
  import Icon from "@iconify/svelte";
</script>

<h1 class="text-2xl font-bold mb-6">Welcome Admin 👋</h1>

<div class="grid grid-cols-1 md:grid-cols-3 gap-6">
  <!-- Users -->
  <div class="bg-white p-6 rounded-lg shadow flex items-center gap-4">
    <Icon icon="mdi:account" class="text-3xl text-blue-600" />
    <div>
      <p class="text-lg font-semibold">120</p>
      <p class="text-gray-500">Users</p>
    </div>
  </div>

  <!-- Reports -->
  <div class="bg-white p-6 rounded-lg shadow flex items-center gap-4">
    <Icon icon="mdi:file-document" class="text-3xl text-green-600" />
    <div>
      <p class="text-lg font-semibold">32</p>
      <p class="text-gray-500">Reports</p>
    </div>
  </div>

  <!-- Revenue -->
  <div class="bg-white p-6 rounded-lg shadow flex items-center gap-4">
    <Icon icon="mdi:currency-usd" class="text-3xl text-yellow-600" />
    <div>
      <p class="text-lg font-semibold">$4,500</p>
      <p class="text-gray-500">Revenue</p>
    </div>
  </div>
</div>
