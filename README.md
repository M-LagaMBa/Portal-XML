# Portal Cilia — Envio de XML

Portal web para recebimento de arquivos XML de notas fiscais durante atendimentos da Cilia.

## Objetivo

Facilitar o envio de um ou mais arquivos XML pelo usuário, permitindo que a equipe responsável receba os documentos para análise e continuidade do atendimento.

## Funcionalidades

- CNPJ e nome da empresa emitente exibidos após a seleção do XML e preenchidos automaticamente a partir do emitente.
- CNPJ da empresa emitente somente para leitura.
- Botão para editar o CNPJ quando for necessário corrigir o dado extraído do XML, com aviso em caso de divergência.
- Identificação dos arquivos com CNPJ de emitente diferente, exibindo nome e CNPJ; o envio desses arquivos exige confirmação explícita.
- Nome da empresa emitente opcional.
- Seleção de um ou vários arquivos XML.
- Adição de novos XMLs após a primeira seleção.
- Remoção individual dos arquivos selecionados.
- Aceita somente arquivos com extensão `.xml`.
- Limite de 10 arquivos e 20 MB no total por envio.
- Seleção múltipla e arrastar/soltar.
- Lista visual dos arquivos selecionados.
- Resumo da quantidade e do tamanho total dos arquivos antes do envio.
- Destaque dos campos obrigatórios quando houver erro.
- Mensagens indicando o que está faltando.
- Loading durante o processamento.
- Indicador visual de progresso.
- Mensagens claras de preparação e processamento do envio.
- Confirmação de sucesso somente após resposta positiva do Apps Script.
- Modal de confirmação orientando o usuário a retornar ao WhatsApp.
- Interface responsiva e identidade visual Cilia.

## Fluxo

1. Usuário acessa o portal.
2. Seleciona um ou mais XMLs.
3. O portal exibe os dados da empresa, lê o CNPJ e o nome do emitente no XML e preenche os campos a partir do emitente principal.
4. Se houver XMLs de emitentes diferentes, o portal mostra os arquivos divergentes e pede confirmação antes do envio.
5. Opcionalmente ajusta o nome da empresa.
6. Pode adicionar ou remover arquivos antes do envio.
7. Clica em **Enviar XML**.
8. O portal apresenta o carregamento durante o processamento.
9. Os arquivos são enviados ao Google Apps Script.
10. O backend salva os arquivos no Google Drive.
10. Após confirmação de sucesso, o portal exibe a mensagem final.

## Estrutura

```text
/
├── index.html
└── README.md
```

### `index.html`

Contém a interface, estilos e JavaScript do portal, incluindo seleção múltipla, validações, preparação dos arquivos, comunicação com o Apps Script, loading, progresso e confirmação.

### `Code.gs`

O backend do Google Apps Script é mantido separadamente e não faz parte deste repositório público.

O frontend utiliza:

```javascript
google.script.run.uploadFiles(...)
```

O Apps Script deve disponibilizar uma função `uploadFiles` compatível com os dados enviados pelo portal.

## Integração

O frontend envia ao Apps Script informações equivalentes a:

```javascript
{
  cnpj: "CNPJ informado",
  nome: "Nome informado",
  files: [
    {
      name: "arquivo.xml",
      mimeType: "application/xml",
      base64: "CONTEUDO_BASE64"
    }
  ]
}
```

O backend é responsável por receber os dados, salvar os arquivos no Google Drive e aplicar a regra de nomenclatura definida para o projeto.

O frontend espera uma resposta semelhante a:

```javascript
{
  success: true
}
```

A mensagem de sucesso somente é exibida quando essa confirmação é recebida.

## Nomenclatura dos arquivos

A identificação dos arquivos considera o CNPJ informado pelo usuário.

Exemplo:

```text
XML_00000000000_1
```

A numeração deve ser controlada pelo backend para evitar conflitos quando houver arquivos duplicados.

## Validação

O portal não valida a estrutura interna completa do XML. Ele lê `emit/CNPJ` e `emit/xNome` para preencher os dados da empresa emitente; o CNPJ também é validado no frontend antes do envio.

A validação do frontend verifica apenas:

- CNPJ informado;
- CNPJ com dígitos verificadores válidos;
- pelo menos um XML selecionado;
- extensão `.xml`.
- máximo de 10 XMLs e 20 MB no total.

## Indicador de progresso

A porcentagem exibida durante o envio é **visual/estimada**, pois o `google.script.run` não fornece ao frontend o percentual real do processamento executado no Apps Script.

O fluxo visual apresenta etapas como:

```text
Preparando
    ↓
Preparando arquivos
    ↓
Processando envio
    ↓
100% — Concluído
    ↓
Confirmação
```

A confirmação final depende da resposta positiva do backend.

Se o Apps Script não responder em 60 segundos, o portal informa que o envio pode estar em andamento e orienta o usuário a confirmar com o atendimento antes de tentar novamente.

## Segurança

Este repositório deve conter somente o frontend.

Os XMLs enviados devem ser utilizados exclusivamente para a continuidade do atendimento.

**Não publique no GitHub:**

- `Code.gs` caso contenha informações internas;
- IDs privados de pastas do Google Drive;
- tokens;
- chaves de API;
- credenciais;
- informações de autenticação;
- dados de fornecedores, oficinas ou usuários;
- dados recebidos nos atendimentos.

## Publicação

O frontend pode ser versionado no GitHub.

Entretanto, o `google.script.run` depende do ambiente do Google Apps Script. Se o HTML for hospedado como página estática fora do Apps Script, essa API não estará disponível diretamente.

Nesse cenário, o backend deverá ser disponibilizado por uma interface HTTP/API e o frontend deverá ser adaptado para utilizar, por exemplo, `fetch()`.

## Tecnologias

- HTML5
- CSS3
- JavaScript
- Google Apps Script
- Google Drive

## Desenvolvimento

Para alterar o portal:

1. Edite o `index.html`.
2. Teste a seleção de XML.
3. Teste múltiplos XMLs.
4. Teste a adição de novos arquivos.
5. Teste a remoção individual.
6. Teste os campos obrigatórios.
7. Teste o envio integrado ao Apps Script.
8. Confirme o comportamento de sucesso e erro.
9. Publique uma nova versão após a validação.

## Status

**Funcional / em evolução.**

O portal possui o fluxo de seleção, preparação e envio de múltiplos XMLs integrado ao Google Apps Script.

## Licença

Projeto de uso interno. Não distribuir ou reutilizar sem autorização.
