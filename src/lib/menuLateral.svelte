<script lang="ts">
  import { page } from '$app/stores';

  type Item = {
    label: string;
    link: string;
  };

  const items: Item[] = [
    { label: 'Home', link: '/main' },
    { label: 'Meus Imóveis', link: '/imoveis' },
  ];
</script>

<style>
  .sidebar {
    position: fixed;
    top: 60px;
    left: 0;
    height: 100%;
    width: 250px;
    background-color: #2c3e50;
    padding: 20px;
    color: white;
    display: flex;
    flex-direction: column;
    align-items: start;
    box-shadow: 2px 0px 10px rgba(0, 0, 0, 0.2);
  }

  .sidebar a {
    color: white;
    text-decoration: none;
    padding: 10px 0;
    width: 100%;
    text-align: left;
    font-size: 18px;
  }

  .sidebar a:hover {
    background-color: #34495e;
  }

  .active-link {
    font-weight: bold;
  }

  .content {
    margin-left: 370px; /* Espaço para o menu lateral */
    padding: 20px;
  }
</style>

<div class="sidebar">
  {#each items as item}
    <a
      href={item.link}
      class={($page.url.pathname === item.link ? 'active-link' : '')}
    >
      {item.label}
    </a>
  {/each}
</div>

<div class="content">
  <slot></slot>
</div>
