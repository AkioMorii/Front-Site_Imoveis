<script lang="ts">
    import '../../styles/global.css';
    import { onMount } from 'svelte';
    import Sidebar from '$lib/menuLateral.svelte';
    import Header from '$lib/header.svelte';

    interface Imovel {
        id?: number;
        local: string;
        cep: string;
        descricao: string;
        contrato: string | null;
    }

    let imoveis: Imovel[] = [];
    let verImoveisVisualizacao = false;  // Flag para mostrar os dados
    let imovelSelecionado: Imovel | null = null;
    let modalVisivel = false;
    let novoImovel: Imovel = { local: '', cep: '', descricao: '', contrato: null };
    let confirmExclusao: boolean = false;
    let imovelParaExcluir: Imovel | null = null;

    onMount(async () => {
        try {
            const response = await fetch('http://localhost:8000/imoveis');
            const data = await response.json();
            
            if (data.imoveis) {
                imoveis = data.imoveis.map((imovel: { local: string, cep: string, descricao: string, contrato_Id: string | null, imovel_id: number }) => ({
                    id: imovel.imovel_id,
                    local: imovel.local,
                    cep: imovel.cep,
                    descricao: imovel.descricao,
                    contrato: imovel.contrato_Id
                }));
            } else {
                console.error('Erro ao buscar imóveis:', data.erro);
            }
        } catch (error) {
            console.error('Erro de conexão:', error);
        }
    });

    // Função para visualizar os detalhes do imóvel
    function verImoveis(imovel: Imovel) {
        imovelSelecionado = imovel;
        verImoveisVisualizacao = true;
    }

    // Função para abrir o modal de criação de imóvel
    function abrirModalCriar() {
        modalVisivel = true;
        novoImovel = { local: '', cep: '', descricao: '', contrato: null }; // Reseta o formulário
    }

    // Função para abrir o modal de edição de imóvel
    function abrirModalEditar(imovel: Imovel) {
        modalVisivel = true;
        novoImovel = { ...imovel }; // Preenche o formulário com os dados do imóvel
    }

    // Função para fechar o modal
    function closeModal() {
        modalVisivel = false;
    }

    // Função para criar ou editar imóvel
    async function salvarImovel() {
        try {
            const method = novoImovel.id ? 'PUT' : 'POST';
            const url = novoImovel.id ? `http://localhost:8000/imoveis/${novoImovel.id}` : 'http://localhost:8000/imoveis';
            const response = await fetch(url, {
                method,
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(novoImovel)
            });

            const data = await response.json();

            if (response.ok) {
                if (!novoImovel.id) {
                    imoveis.push(data.imovel);
                } else {
                    const index = imoveis.findIndex((imovel) => imovel.id === novoImovel.id);
                    if (index !== -1) imoveis[index] = data.imovel;
                }

                closeModal(); // Fecha o modal
            } else {
                alert(data.erro || 'Erro ao salvar imóvel.');
            }
        } catch (error) {
            console.error('Erro ao salvar imóvel:', error);
        }
    }

    // Função para excluir imóvel
    async function excluirImovel() {
        if (imovelParaExcluir) {
            try {
                const response = await fetch(`http://localhost:8000/imoveis/${imovelParaExcluir.id}`, {
                    method: 'DELETE',
                });

                if (response.ok) {
                    imoveis = imoveis.filter((imovel) => imovel.id !== imovelParaExcluir.id);
                    confirmExclusao = false;
                    imovelParaExcluir = null;
                } else {
                    alert('Erro ao excluir imóvel.');
                }
            } catch (error) {
                console.error('Erro ao excluir imóvel:', error);
            }
        }
    }

    // Função para confirmar a exclusão
    function confirmarExclusao(imovel: Imovel) {
        confirmExclusao = true;
        imovelParaExcluir = imovel;
    }

    // Função para cancelar a exclusão
    function cancelarExclusao() {
        confirmExclusao = false;
        imovelParaExcluir = null;
    }
</script>

<Header/>
<Sidebar>
    <div class="tbl">
        <table>
            <thead>
                <tr>
                    <th>Local</th>
                    <th>Cep</th>
                    <th>Alugado</th>
                    <th>Descrição</th>
                    <th>Editar</th>
                    <th>Excluir</th>
                </tr>
            </thead>
            <tbody>
                {#each imoveis as imovel}
                    <tr>
                        <td style="cursor: pointer" on:click={() => verImoveis(imovel)}>{imovel.local}</td>
                        <td>{imovel.cep}</td>
                        <td>
                            {#if imovel.contrato}
                                Sim
                            {:else}
                                Não
                            {/if}
                        </td>
                        <td>{imovel.descricao}</td>
                        <td>
                            <button on:click={() => abrirModalEditar(imovel)}>Editar</button>
                        </td>
                        <td>
                            <button on:click={() => confirmarExclusao(imovel)}>Excluir</button>
                        </td>
                    </tr>
                {/each}
            </tbody>
        </table>
    </div>

    <!-- Modal para confirmação de exclusão -->
    {#if confirmExclusao}
        <div class="modal">
            <div class="modal-content">
                <h3>Tem certeza que deseja excluir este imóvel?</h3>
                <button on:click={excluirImovel}>Sim, excluir</button>
                <button on:click={cancelarExclusao}>Cancelar</button>
            </div>
        </div>
    {/if}

</Sidebar>
