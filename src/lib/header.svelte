<!-- src/components/Header.svelte -->
<script>
  export let userName = "Usuário";
</script>


<style>
  
.header {
  position:fixed;
  left: 0;
  top:0;
  width: 100%;
  height: 28px;
  z-index: 10;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  background-color: #324558;
  color: white;
  font-family: Arial, sans-serif;
  font-size: 1.2rem;
}

  .user-name {
    padding-right: 100px;
    text-align: right;
    font-style: italic;
  }

  .content_header {
    margin-top:60px;
  }
</style>


<header class="header">
  <h3>Site imóveis</h3>
  <div class="user-name">{userName}</div>
</header>

<div class="content_header">
</div>
