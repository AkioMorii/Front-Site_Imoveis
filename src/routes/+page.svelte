<style>
 .main {
    height: 100vh;
  }

  .form_Container_Login{
    width: 400px;
    height: 50%;
  }

  input[type="text"],
  input[type="password"] {
    margin: 60px 10px 0px 10px;
  }

  button[type="submit"]{
    margin-top: 50px;
  }

</style>

<script>
  import { goto } from '$app/navigation';  // Para navegação entre páginas
  import '../styles/global.css';

  let login = '';
  let senha = '';
  let mensagem = '';
  let errorMessage = '';
  

  async function tentarLogin() {
    const res = await fetch('http://localhost:8000/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ login, senha })
    });

    const data = await res.json();
    mensagem = data.message;
    alert(mensagem)

    //redirecionamento para main:
    if (data.message && data.message.includes('Autenticado')) {
      const usuario = {
        id: data.usuario.id,
        name: data.usuario.name
    };

      sessionStorage.setItem('usuario', JSON.stringify(usuario));
      goto('/main');
             
    } else {
      errorMessage = data.message || 'Erro desconhecido';
    }
  }
</script>
  
<div class="main center">
  <div class= "form_Container_Login">

    <div class="center">
      <h2>Login</h2>
    </div>

    <form class="center" on:submit|preventDefault={tentarLogin}>

      <input type="text" bind:value={login} placeholder="Login" />
      <input type="password" bind:value={senha} placeholder="Senha" />
      {#if errorMessage}
        <div class="error">{errorMessage}</div>
      {/if}
      <button type="submit">Entrar</button>

    </form>
  </div>
</div>

