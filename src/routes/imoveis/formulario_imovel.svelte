<script>
    
    export let imovelSelecionado: {
        local: string;
        cep: string;
        descricao: string;
        contrato: string | null;
    };
    
    export let closeModal: () => void;  // Função para fechar o formulário
</script>

<div class="form_Container_CRUD">
    <div class="form_CRUD">
        <button class="form_Close_Button" on:click={closeModal}>✖</button>

        <form>
            <p class="mb-2 font-semibold">Editar Imóvel</p>
            <label class="block mb-3">
                Local:
                <input
                    type="text"
                    class="border border-gray-300 p-1 w-full mt-1"
                    bind:value={imovelSelecionado.local}  // Liga o input ao imóvel
                />
            </label>
            <label class="block mb-3">
                CEP:
                <input
                    type="text"
                    class="border border-gray-300 p-1 w-full mt-1"
                    bind:value={imovelSelecionado.cep}  // Liga o input ao imóvel
                />
            </label>
            <label class="block mb-3">
                Descrição:
                <textarea
                    class="border border-gray-300 p-1 w-full mt-1"
                    bind:value={imovelSelecionado.descricao}  // Liga o textarea ao imóvel
                ></textarea>
            </label>
            <label class="block mb-3">
                Alugado:
                <input
                    type="checkbox"
                    bind:checked={imovelSelecionado.contrato}  // Liga o checkbox ao imóvel
                />
            </label>
            <button type="submit" class="bg-blue-500 text-white px-4 py-1 rounded">Salvar</button>
        </form>
    </div>
</div>
