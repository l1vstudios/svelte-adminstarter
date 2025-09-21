<script lang="ts">
  import Icon from "@iconify/svelte";
  import SidebarMenu from "./SidebarMenu.svelte";
  import "../app.css";

  import Dashboard from "../Components/Dashboard.svelte";
  import Laundry from "../Components/Laundry.svelte";
  import Orders from "../Components/Orders.svelte";

  type PageName = "Dashboard" | "Laundry" | "Orders";

  let sidebarOpen = false;
  let collapsed = false;
  let activePage: PageName = "Dashboard";

  const pages: Record<PageName, any> = {
    Dashboard,
    Laundry,
    Orders,
  };

  function toggleSidebar() {
    collapsed = !collapsed;
  }

  function setPage(event: CustomEvent<PageName>) {
    activePage = event.detail;
  }
</script>

<div class="flex bg-gray-50">
  <aside
    class="hidden md:flex fixed left-0 top-0 h-screen flex-col
           bg-gradient-to-b from-blue-900 to-blue-950 text-white
           transition-all duration-300 z-50"
    class:w-64={!collapsed}
    class:w-16={collapsed}
  >
    <div class="flex flex-col h-full p-4">
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
      class="fixed z-50 flex flex-col w-64 h-full
             bg-gradient-to-b from-blue-900 to-blue-950 text-white
             p-4 md:hidden"
    >
      <SidebarMenu
        collapsed={false}
        {activePage}
        toggleSidebar={() => (sidebarOpen = false)}
        on:selectPage={setPage}
      />
    </aside>
  {/if}

  <div
    class="flex-1 flex flex-col min-h-screen transition-all duration-300"
    class:md:ml-64={!collapsed}
    class:md:ml-16={collapsed}
  >
    <header
      class="bg-white shadow px-6 py-4 flex items-center sticky top-0 z-40"
      class:justify-between={true}
      class:md:justify-center={collapsed}
    >
      <button
        class="md:hidden"
        on:click={() => (sidebarOpen = !sidebarOpen)}
        aria-label="Open sidebar"
      >
        <Icon icon="mdi:menu" class="text-2xl text-gray-600" />
      </button>

      <h1 class="text-lg font-semibold">{activePage}</h1>

      <div class="md:hidden w-8"></div>
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
