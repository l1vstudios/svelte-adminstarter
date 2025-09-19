<script lang="ts">
  import Icon from "@iconify/svelte";
  import { createEventDispatcher } from "svelte";

  export let collapsed: boolean = false;
  export let toggleSidebar: () => void;
  export let activePage: string = "Dashboard";

  const dispatch = createEventDispatcher();

  function selectPage(page: string) {
    dispatch("selectPage", page);
  }
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
      <button
        on:click={() => selectPage("Dashboard")}
        class="flex items-center gap-3 px-3 py-2 rounded-lg w-full text-left"
        class:bg-gray-100={activePage === "Dashboard"}
        class:text-gray-900={activePage === "Dashboard"}
        class:hover:bg-gray-100={activePage !== "Dashboard"}
        class:justify-center={collapsed}
        title={collapsed ? "Dashboard" : ""}
      >
        <Icon icon="mdi:view-dashboard" class="text-lg flex-shrink-0" />
        {#if !collapsed}
          <span>Dashboard</span>
        {/if}
      </button>

      <button
        on:click={() => selectPage("Product")}
        class="flex items-center gap-3 px-3 py-2 rounded-lg w-full text-left"
        class:bg-gray-100={activePage === "Product"}
        class:text-gray-900={activePage === "Product"}
        class:hover:bg-gray-100={activePage !== "Product"}
        class:justify-center={collapsed}
        title={collapsed ? "Product" : ""}
      >
        <Icon icon="mdi:cube-outline" class="text-lg flex-shrink-0" />
        {#if !collapsed}
          <span>Product</span>
        {/if}
      </button>

      <button
        on:click={() => selectPage("Orders")}
        class="flex items-center gap-3 px-3 py-2 rounded-lg w-full text-left"
        class:bg-gray-100={activePage === "Orders"}
        class:text-gray-900={activePage === "Orders"}
        class:hover:bg-gray-100={activePage !== "Orders"}
        class:justify-center={collapsed}
        title={collapsed ? "Orders" : ""}
      >
        <Icon icon="mdi:cart-outline" class="text-lg flex-shrink-0" />
        {#if !collapsed}
          <span>Orders</span>
        {/if}
      </button>

      <button
        on:click={() => selectPage("Customers")}
        class="flex items-center gap-3 px-3 py-2 rounded-lg w-full text-left"
        class:bg-gray-100={activePage === "Customers"}
        class:text-gray-900={activePage === "Customers"}
        class:hover:bg-gray-100={activePage !== "Customers"}
        class:justify-center={collapsed}
        title={collapsed ? "Customers" : ""}
      >
        <Icon icon="mdi:account-group-outline" class="text-lg flex-shrink-0" />
        {#if !collapsed}
          <span>Customers</span>
        {/if}
      </button>

      <button
        on:click={() => selectPage("Chat")}
        class="flex items-center px-3 py-2 rounded-lg w-full text-left relative"
        class:bg-gray-100={activePage === "Chat"}
        class:text-gray-900={activePage === "Chat"}
        class:hover:bg-gray-100={activePage !== "Chat"}
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
      </button>
    </nav>
  </div>

  <div class="mb-6 px-2">
    {#if !collapsed}
      <h2 class="text-xs font-semibold text-gray-400 mb-2">OTHER</h2>
    {/if}
    <nav class="space-y-1">
      <button
        on:click={() => selectPage("Email")}
        class="flex items-center gap-3 px-3 py-2 rounded-lg w-full text-left"
        class:bg-gray-100={activePage === "Email"}
        class:text-gray-900={activePage === "Email"}
        class:hover:bg-gray-100={activePage !== "Email"}
        class:justify-center={collapsed}
        title={collapsed ? "Email" : ""}
      >
        <Icon icon="mdi:email-outline" class="text-lg flex-shrink-0" />
        {#if !collapsed}
          <span>Email</span>
        {/if}
      </button>

      <button
        on:click={() => selectPage("Analytics")}
        class="flex items-center gap-3 px-3 py-2 rounded-lg w-full text-left"
        class:bg-gray-100={activePage === "Analytics"}
        class:text-gray-900={activePage === "Analytics"}
        class:hover:bg-gray-100={activePage !== "Analytics"}
        class:justify-center={collapsed}
        title={collapsed ? "Analytics" : ""}
      >
        <Icon icon="mdi:chart-line" class="text-lg flex-shrink-0" />
        {#if !collapsed}
          <span>Analytics</span>
        {/if}
      </button>

      <button
        on:click={() => selectPage("Integration")}
        class="flex items-center gap-3 px-3 py-2 rounded-lg w-full text-left"
        class:bg-gray-100={activePage === "Integration"}
        class:text-gray-900={activePage === "Integration"}
        class:hover:bg-gray-100={activePage !== "Integration"}
        class:justify-center={collapsed}
        title={collapsed ? "Integration" : ""}
      >
        <Icon icon="mdi:link-variant" class="text-lg flex-shrink-0" />
        {#if !collapsed}
          <span>Integration</span>
        {/if}
      </button>

      <button
        on:click={() => selectPage("Performance")}
        class="flex items-center gap-3 px-3 py-2 rounded-lg w-full text-left"
        class:bg-gray-100={activePage === "Performance"}
        class:text-gray-900={activePage === "Performance"}
        class:hover:bg-gray-100={activePage !== "Performance"}
        class:justify-center={collapsed}
        title={collapsed ? "Performance" : ""}
      >
        <Icon icon="mdi:rocket-launch" class="text-lg flex-shrink-0" />
        {#if !collapsed}
          <span>Performance</span>
        {/if}
      </button>
    </nav>
  </div>

  <!-- ACCOUNT -->
  <div class="mt-auto px-2">
    {#if !collapsed}
      <h2 class="text-xs font-semibold text-gray-400 mb-2">ACCOUNT</h2>
    {/if}
    <nav class="space-y-1">
      <button
        on:click={() => selectPage("Account")}
        class="flex items-center gap-3 px-3 py-2 rounded-lg w-full text-left"
        class:bg-gray-100={activePage === "Account"}
        class:text-gray-900={activePage === "Account"}
        class:hover:bg-gray-100={activePage !== "Account"}
        class:justify-center={collapsed}
        title={collapsed ? "Account" : ""}
      >
        <Icon icon="mdi:account-outline" class="text-lg flex-shrink-0" />
        {#if !collapsed}
          <span>Account</span>
        {/if}
      </button>

      <button
        on:click={() => selectPage("Members")}
        class="flex items-center gap-3 px-3 py-2 rounded-lg w-full text-left"
        class:bg-gray-100={activePage === "Members"}
        class:text-gray-900={activePage === "Members"}
        class:hover:bg-gray-100={activePage !== "Members"}
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
      </button>
    </nav>
  </div>
</div>
