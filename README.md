# Lobby Cypress Tests

Repositório contendo a suíte de testes automatizados com Cypress de o projeto **Lobby** ou **Dev**.

## Visão Geral

Este projeto utiliza o framework de testes E2E (End-to-End) Cypress para verificar o comportamento e a funcionalidade da aplicação **Lob**. A suíte de testes foi projetada para ser robusta, confiável e eficiente, garantindo que as funcionalidades essenciais do projeto sejam testadas automaticamente.

## Estrutura do Projeto

- **cypress/**: Contém a pasta com todos os testes, fixtures e comandos customizados.
  - **fixtures/**: Armazena arquivos de dados para uso nos testes.
  - **integration/**: Contém os arquivos dos testes automatizados.
  - **plugins/**: Scripts de configuração e customizações do Cypress.
  - **support/**: Funções e comandos globais que podem ser usados por todos os testes.
- **cypress.json**: Arquivo de configuração do Cypress.
- **README.md**: Documentação do projeto.

## Requisitos

- Node.js (versão 14 ou superior)
- Cypress (versão 12 ou superior)

## Instalação

1. Clone o repositório:

   ```bash
   git clone https://github.com/GbrlSouza/LobCypressTestsLocalHost.git
   ```

2. Acesse o diretório do projeto:

   ```bash
   cd LobCypressTestsLocalHost
   ```

3. Instale as dependências:

   ```bash
   npm install
   ```

## Executando os Testes

- Para abrir o Cypress em modo interativo:

  ```bash
  npx cypress open
  ```

- Para executar os testes no modo headless:

  ```bash
  npx cypress run
  ```

## Configuração

- As configurações de ambiente podem ser ajustadas no arquivo `cypress.json`.
- Adicione credenciais sensíveis ou variáveis de ambiente de forma segura, se necessário.

## Estrutura de Testes

- **Testes de Login**: Verificam a funcionalidade de autenticação do usuário.
- **Testes de UI**: Validam elementos da interface, como botões, formulários e links.
- **Testes de Funcionalidade**: Checam funcionalidades críticas, como envio de formulários e navegação.

## Boas Práticas

- Sempre mantenha os testes atualizados com as funcionalidades da aplicação.
- Use comandos customizados no Cypress para evitar repetição de código.
- Execute os testes em diferentes navegadores e ambientes para melhor cobertura.

## Contribuição

1. Faça um fork do repositório.
2. Crie uma branch para sua feature/bug fix (`git checkout -b minha-branch`).
3. Commit suas mudanças (`git commit -m 'Minha feature incrível'`).
4. Dê push na branch (`git push origin minha-branch`).
5. Abra um Pull Request.

## Licença

Este projeto é licenciado sob a [MIT License](LICENSE).

---

Sinta-se à vontade para contribuir ou abrir issues caso encontre algum problema ou tenha sugestões de melhoria!

## Saiba Mais

Esse arquivo é um **workflow do GitHub Actions** que automatiza a execução de testes com o **Cypress** sempre que houver um *push* (envio de código) para o repositório. Vou explicar cada parte em detalhes:

### Estrutura do Workflow
- **Nome do Workflow: `Cypress Tests`**
  - Nome que aparece no GitHub para identificar o que o workflow faz.

### Quando Executar o Workflow
- **on: push**
  - Significa que o workflow será acionado automaticamente quando houver um *push* no repositório. Pode ser configurado para outros eventos, mas aqui ele está configurado para mudanças no código.

---

### Definição do Trabalho (Job)
- **jobs: cypress-run**
  - Define um trabalho chamado `cypress-run`, que será executado em uma máquina Ubuntu.

### Ambiente de Execução
- **runs-on: ubuntu-22.04**
  - Especifica que o trabalho será executado em um ambiente Linux (Ubuntu 22.04). É uma das opções mais comuns para CI/CD porque é rápida e amplamente compatível.

---

### Passos do Workflow
1. **Checkout do Código**
   - **name: Checkout**
   - **uses: actions/checkout@v4**
   - Este passo usa a ação oficial `actions/checkout` para clonar o código do repositório. É necessário para que o código esteja disponível no ambiente de execução.

2. **Instalar Dependências**
   - **name: Install Dependencies**
   - **run: npm install**
   - Executa `npm install` para baixar todas as dependências listadas no `package.json` do projeto. Isso garante que o Cypress e qualquer outra biblioteca estejam instalados antes de rodar os testes.

3. **Corrigir Permissões do Cypress**
   - **name: Fix Cypress Permissions**
   - **run: chmod +x ./node_modules/.bin/cypress**
   - Altera as permissões do binário do Cypress para que ele seja executável. Isso é feito com o comando `chmod +x`, que pode ser necessário em ambientes Linux.

4. **Instalar Cypress**
   - **name: Install Cypress**
   - **run: npx cypress install**
   - Usa `npx` para instalar o binário do Cypress. Isso garante que o Cypress esteja configurado corretamente e pronto para rodar os testes.

5. **Executar Testes Cypress**
   - **name: Run Cypress Tests**
   - **run: npm run test**
   - Executa o comando `npm run test`, que deve estar configurado no `package.json` para rodar os testes Cypress. Aqui, espera-se que o Cypress inicie e execute todos os testes definidos.

6. **Upload de Screenshots do Cypress**
   - **name: Upload Cypress Screenshots**
   - **if: failure()**
   - **uses: actions/upload-artifact@v3**
   - Esse passo faz o upload das capturas de tela (screenshots) geradas pelo Cypress, mas **somente se os testes falharem**. A condição `if: failure()` garante que os screenshots sejam salvos apenas em caso de falha, ajudando a depurar problemas.
   - **with**: Define o nome do artefato (`cypress-screenshots`) e o caminho para encontrar os screenshots (`cypress/screenshots`). Se nenhum arquivo for encontrado, ele ignora.

7. **Upload de Vídeos do Cypress**
   - **name: Upload Cypress Videos**
   - **uses: actions/upload-artifact@v3**
   - Sem a condição de falha, este passo faz o upload dos vídeos gerados pelo Cypress, independentemente de os testes passarem ou falharem.
   - **with**: Define o nome do artefato (`cypress-videos`) e o caminho para encontrar os vídeos (`cypress/videos`). Também ignora se não houver arquivos.

---

### Resumo
Esse workflow configura o ambiente, instala as dependências, garante que o Cypress esteja pronto para uso, e executa os testes. Se os testes falharem, ele armazena screenshots e vídeos como artefatos, facilitando a análise do que deu errado. É uma configuração robusta para integrar testes Cypress em um processo de CI/CD.

### Explicando o Workflow YAML
Aqui está o que acontece em cada etapa:

1. **name: Cypress Tests**: Define o nome do workflow. Este é um identificador simples para o que o workflow faz.

2. **on: push**: O workflow será executado sempre que houver um `push` para o repositório, ou seja, quando houver mudanças no código.

3. **jobs: cypress-run**: Define o trabalho chamado `cypress-run` que será executado em um sistema operacional Ubuntu 22.04.

### Etapas do Workflow
1. **Checkout**:
   - Usa a ação `actions/checkout@v4` para baixar o código do repositório no runner (o ambiente que executa o workflow).

2. **Install Dependencies**:
   - Roda `npm install` para instalar as dependências listadas no `package.json`.

3. **Fix Cypress Permissions**:
   - Executa `chmod +x ./node_modules/.bin/cypress` para garantir que o Cypress tenha as permissões corretas para ser executado.

4. **Install Cypress**:
   - Roda `npx cypress install` para baixar e instalar os binários do Cypress.

5. **Run Cypress Tests**:
   - Executa `npm run test`, que inicia os testes Cypress definidos no `package.json`.

6. **List Cypress Folder**:
   - Executa `ls -la cypress/videos` para listar os arquivos na pasta de vídeos, garantindo que os arquivos foram gerados corretamente. Isso é útil para depuração, especialmente se você quiser verificar se os arquivos esperados foram criados.

7. **Upload Cypress Screenshots**:
   - Se os testes falharem, esta etapa faz upload das capturas de tela como artefatos. O `if: failure()` garante que esta etapa só seja executada se houver falhas.

8. **Upload Cypress Videos**:
   - Faz upload dos vídeos gravados dos testes Cypress como artefatos. Se não houver arquivos, o workflow ignora silenciosamente.

---

### Comtinua não entendendo!?
- Passa no RH!
