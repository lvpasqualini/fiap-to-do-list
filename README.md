# To-Do List - Kotlin Jetpack Compose

## Descrição e Objetivo
Este projeto é um aplicativo Android de Lista de Tarefas (To-Do List) desenvolvido como parte de uma atividade acadêmica. O objetivo principal da aplicação é implementar a camada de apresentação (UI) utilizando Jetpack Compose, a navegação entre telas e a integração com a arquitetura de persistência de dados local (Room) já existente. O aplicativo permite ao usuário listar, criar, editar, marcar como concluída e excluir tarefas.

## Tecnologias Utilizadas
* **Kotlin**: Linguagem de programação principal.
* **Jetpack Compose**: Kit de ferramentas moderno para construção de UI nativa de forma declarativa.
* **Room**: Biblioteca de persistência para abstração do banco de dados SQLite.
* **Coroutines e Flow**: Para programação assíncrona e fluxos de dados reativos.
* **ViewModel**: Gerenciamento de dados relacionados à UI com consciência do ciclo de vida.
* **Navigation Compose**: Roteamento e navegação fluida entre telas no Compose.

## Arquitetura e Componentes

### Responsabilidade de `TarefaRepository`
A camada de repositório atua como a única fonte da verdade para os dados da aplicação. Ele isola a origem dos dados (neste caso, o `TarefaDAO` utilizando o Room) do restante do aplicativo, fornecendo uma API limpa para acesso, inserção, atualização e exclusão de tarefas.

### Responsabilidade de `TarefaViewModel`
A `TarefaViewModel` é responsável por preparar e gerenciar os dados para a interface de usuário. Ela interage com o `TarefaRepository` para realizar as operações de negócio e de banco de dados, além de expor a lista de tarefas como um estado observável (como `StateFlow`). Isso garante que a UI reaja automaticamente a mudanças nos dados e que essas informações sobrevivam a mudanças de configuração (como a rotação do dispositivo).

### Como `ListaTarefasScreen` observa o estado e dispara ações
A tela `ListaTarefasScreen` utiliza uma `LazyColumn` para renderizar as tarefas de maneira eficiente. Ela se inscreve e observa o estado exposto pela `TarefaViewModel`. Quando o usuário interage com a interface (por exemplo, clicando em concluir, editar ou excluir), a tela não altera os dados diretamente; em vez disso, ela dispara eventos ou chamadas de função para a ViewModel. A ViewModel processa a lógica e atualiza o estado, forçando a recomposição reativa da UI.

### Como `FormularioTarefaScreen` diferencia cadastro e edição
A tela `FormularioTarefaScreen` atende aos dois cenários utilizando a mesma interface. A diferenciação é feita através da presença (ou ausência) de um identificador (`ID` da tarefa) passado como argumento de navegação. Se um ID válido for fornecido, a ViewModel busca a tarefa e o formulário é pré-preenchido com os dados existentes (modo de edição). Caso contrário, o formulário é iniciado vazio (modo de criação).

### Rotas em `AppNavigation` e passagem do ID da tarefa
O componente `AppNavigation` centraliza o roteamento utilizando o Navigation Compose. Estão configuradas duas rotas principais:
1. Rota principal para a lista de tarefas (ex: `"lista"`).
2. Rota para o formulário (ex: `"formulario?tarefaId={tarefaId}"`).
A passagem do ID é feita pela URL da rota como um argumento, permitindo que a tela de destino receba essa informação e busque a tarefa correspondente no banco.

### Criação da ViewModel e navegação na `MainActivity`
A `MainActivity` atua como o ponto de entrada da interface. Nela, a `TarefaViewModel` é instanciada utilizando a sua Factory (garantindo a injeção do repositório/banco de dados). Após a instância, a `MainActivity` inicia o conteúdo definindo o `AppNavigation` dentro do bloco `setContent`, assumindo o controle total sobre a exibição das telas e substituindo qualquer interface de template padrão.

## Instruções para Executar o Projeto
1. Clone este repositório para a sua máquina local.
2. Abra o projeto no **Android Studio**.
3. Aguarde o Gradle sincronizar todas as dependências (`Sync Project with Gradle Files`).
4. Compile e execute o projeto clicando em `Run 'app'` utilizando um emulador ou dispositivo físico Android.

## Evidências

- **Tela inicial com a lista de tarefas:**
<img width="400" height="400" alt="minhas tarefas vazia" src="https://github.com/user-attachments/assets/3a347fb7-88d9-422d-8fa9-c5bc5abfed4c" />

- **Formulário vazio:**
<img width="400" height="400" alt="formulario criar tarefa" src="https://github.com/user-attachments/assets/fae00f14-2952-4ff0-819d-63a6d3595bb2" />

- **Cadastro de uma nova tarefa:**
<img width="400" height="400" alt="criando tarefa" src="https://github.com/user-attachments/assets/9d8dbb92-e42a-4730-bda1-3680790e92b1" />

- **Tarefa cadastrada aparecendo na lista:**
<img width="400" height="400" alt="lista minhas tarefas" src="https://github.com/user-attachments/assets/d1eab020-0c79-47fa-8f28-0f1537dbd6a3" />

- **Edição de uma tarefa existente:**
<img width="280" height="400" alt="editando tarefa" src="https://github.com/user-attachments/assets/713bb7c4-6a56-4659-b0fc-40666f7e7a36" />

- **Tarefa marcada como concluída:**
<img width="280" height="400" alt="tarefa concluida" src="https://github.com/user-attachments/assets/70457bb0-cc94-47e4-96c4-8308b23028e7" />

- **Exclusão de uma tarefa:**
<img width="400" height="400" alt="tarefa excluida" src="https://github.com/user-attachments/assets/6dc676ab-9752-45d3-851f-a91f4da1965a" />

- **Build:**
<img width="1600" height="696" alt="build" src="https://github.com/user-attachments/assets/2719c1d5-caff-4645-8f31-f015f8a1a974" />

