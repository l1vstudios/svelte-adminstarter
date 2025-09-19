<script lang="ts">
  import Icon from "@iconify/svelte";
  import SidebarMenu from "./SidebarMenu.svelte";
  import "../app.css";

  import Dashboard from "../Components/Dashboard.svelte";
  import Product from "../Components/Product.svelte";

  type PageName = "Dashboard" | "Product";

  let sidebarOpen = false;
  let collapsed = false;
  let activePage: PageName = "Dashboard";

  const pages: Record<PageName, any> = {
    Dashboard,
    Product,
  };

  function toggleSidebar() {
    collapsed = !collapsed;
  }

  function setPage(event: CustomEvent<PageName>) {
    activePage = event.detail;
  }
</script>

<div class="flex h-screen bg-gray-50">
  <aside
    class="hidden md:flex flex-col bg-white border-r transition-all duration-300"
    class:w-64={!collapsed}
    class:w-16={collapsed}
  >
    <div class="flex flex-col h-full text-gray-700 p-4">
      <SidebarMenu
        {collapsed}
        {activePage}
        {toggleSidebar}
        on:selectPage={setPage}
      />
    </div>
  </aside>

  {#if sidebarOpen}
    <button
      class="fixed inset-0 z-40 bg-black bg-opacity-30 md:hidden"
      aria-label="Close sidebar"
      on:click={() => (sidebarOpen = false)}
    ></button>

    <aside
      class="fixed z-50 flex flex-col w-64 h-full bg-white border-r p-4 md:hidden"
    >
      <SidebarMenu
        collapsed={false}
        {activePage}
        toggleSidebar={() => (sidebarOpen = false)}
        on:selectPage={setPage}
      />
    </aside>
  {/if}

  <div class="flex-1 flex flex-col">
    <header class="bg-white shadow px-6 py-4 flex justify-between items-center">
      <button
        class="md:hidden"
        on:click={() => (sidebarOpen = !sidebarOpen)}
        aria-label="Open sidebar"
      >
        <Icon icon="mdi:menu" class="text-2xl text-gray-600" />
      </button>

      <h1 class="text-lg font-semibold">{activePage}</h1>
    </header>

    <main class="flex-1 p-6">
      <svelte:component this={pages[activePage]} />
    </main>
  </div>
</div>

<style>
  aside {
    transition: width 0.3s ease;
  }
</style>
