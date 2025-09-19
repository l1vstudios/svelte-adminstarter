<script lang="ts">
  import Icon from "@iconify/svelte";
  export let collapsed: boolean = false;
  export let toggleSidebar: () => void;
</script>

<div class="flex flex-col h-full text-gray-700">
  <div class="flex items-center justify-between mb-8 px-2">
    {#if !collapsed}
      <div>
        <h1 class="text-lg font-bold">Admin Dashboard.</h1>
        <p class="text-xs text-gray-500">SVELTE TAILWIND</p>
      </div>
    {/if}
    <button
      class="text-gray-500 hover:text-gray-700 p-1"
      aria-label={collapsed ? "Expand sidebar" : "Collapse sidebar"}
      on:click={toggleSidebar}
    >
      <Icon
        icon={collapsed ? "mdi:chevron-right" : "mdi:chevron-left"}
        class="text-2xl"
      />
    </button>
  </div>

  <!-- MAIN MENU -->
  <div class="mb-6 px-2">
    {#if !collapsed}
      <h2 class="text-xs font-semibold text-gray-400 mb-2">MAIN MENU</h2>
    {/if}
    <nav class="space-y-1">
      <a
        href="/"
        class="flex items-center gap-3 px-3 py-2 rounded-lg bg-gray-100 text-gray-900"
        class:justify-center={collapsed}
        title={collapsed ? "Dashboard" : ""}
      >
        <Icon icon="mdi:view-dashboard" class="text-lg flex-shrink-0" />
        {#if !collapsed}
          <span>Dashboard</span>
        {/if}
      </a>
      <a
        href="/products"
        class="flex items-center gap-3 px-3 py-2 rounded-lg hover:bg-gray-100"
        class:justify-center={collapsed}
        title={collapsed ? "Products" : ""}
      >
        <Icon icon="mdi:cube-outline" class="text-lg flex-shrink-0" />
        {#if !collapsed}
          <span>Products</span>
        {/if}
      </a>
      <a
        href="/orders"
        class="flex items-center gap-3 px-3 py-2 rounded-lg hover:bg-gray-100"
        class:justify-center={collapsed}
        title={collapsed ? "Orders" : ""}
      >
        <Icon icon="mdi:cart-outline" class="text-lg flex-shrink-0" />
        {#if !collapsed}
          <span>Orders</span>
        {/if}
      </a>
      <a
        href="/customers"
        class="flex items-center gap-3 px-3 py-2 rounded-lg hover:bg-gray-100"
        class:justify-center={collapsed}
        title={collapsed ? "Customers" : ""}
      >
        <Icon icon="mdi:account-group-outline" class="text-lg flex-shrink-0" />
        {#if !collapsed}
          <span>Customers</span>
        {/if}
      </a>
      <a
        href="/chat"
        class="flex items-center px-3 py-2 rounded-lg hover:bg-gray-100 relative"
        class:justify-center={collapsed}
        title={collapsed ? "Chat (22)" : ""}
      >
        <Icon icon="mdi:chat-outline" class="text-lg flex-shrink-0" />
        {#if !collapsed}
          <span class="ml-3">Chat</span>
          <span class="text-xs bg-gray-200 px-2 py-0.5 rounded-full ml-auto"
            >22</span
          >
        {:else}
          <span
            class="absolute -top-1 -right-1 bg-red-500 text-white text-xs rounded-full w-5 h-5 flex items-center justify-center"
            >22</span
          >
        {/if}
      </a>
    </nav>
  </div>

  <div class="mb-6 px-2">
    {#if !collapsed}
      <h2 class="text-xs font-semibold text-gray-400 mb-2">OTHER</h2>
    {/if}
    <nav class="space-y-1">
      <a
        href="/email"
        class="flex items-center gap-3 px-3 py-2 rounded-lg hover:bg-gray-100"
        class:justify-center={collapsed}
        title={collapsed ? "Email" : ""}
      >
        <Icon icon="mdi:email-outline" class="text-lg flex-shrink-0" />
        {#if !collapsed}
          <span>Email</span>
        {/if}
      </a>
      <a
        href="/analytics"
        class="flex items-center gap-3 px-3 py-2 rounded-lg hover:bg-gray-100"
        class:justify-center={collapsed}
        title={collapsed ? "Analytics" : ""}
      >
        <Icon icon="mdi:chart-line" class="text-lg flex-shrink-0" />
        {#if !collapsed}
          <span>Analytics</span>
        {/if}
      </a>
      <a
        href="/integration"
        class="flex items-center gap-3 px-3 py-2 rounded-lg hover:bg-gray-100"
        class:justify-center={collapsed}
        title={collapsed ? "Integration" : ""}
      >
        <Icon icon="mdi:link-variant" class="text-lg flex-shrink-0" />
        {#if !collapsed}
          <span>Integration</span>
        {/if}
      </a>
      <a
        href="/performance"
        class="flex items-center gap-3 px-3 py-2 rounded-lg hover:bg-gray-100"
        class:justify-center={collapsed}
        title={collapsed ? "Performance" : ""}
      >
        <Icon icon="mdi:rocket-launch" class="text-lg flex-shrink-0" />
        {#if !collapsed}
          <span>Performance</span>
        {/if}
      </a>
    </nav>
  </div>

  <!-- ACCOUNT -->
  <div class="mt-auto px-2">
    {#if !collapsed}
      <h2 class="text-xs font-semibold text-gray-400 mb-2">ACCOUNT</h2>
    {/if}
    <nav class="space-y-1">
      <a
        href="/account"
        class="flex items-center gap-3 px-3 py-2 rounded-lg hover:bg-gray-100"
        class:justify-center={collapsed}
        title={collapsed ? "Account" : ""}
      >
        <Icon icon="mdi:account-outline" class="text-lg flex-shrink-0" />
        {#if !collapsed}
          <span>Account</span>
        {/if}
      </a>
      <a
        href="/members"
        class="flex items-center gap-3 px-3 py-2 rounded-lg hover:bg-gray-100"
        class:justify-center={collapsed}
        title={collapsed ? "Members" : ""}
      >
        <Icon
          icon="mdi:account-multiple-outline"
          class="text-lg flex-shrink-0"
        />
        {#if !collapsed}
          <span>Members</span>
        {/if}
      </a>
    </nav>
  </div>
</div>
