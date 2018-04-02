---
title: Documentação da API - Gensoft

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='mailto:suporte@genomika.com.br'>Dúvidas ? Fale conosco</a>

includes:
  - errors

search: true
---

# Introdução

Este documento visa apresentar a documentação de uso da API do GENSOFT. Nesta API apresentamos os endpoints necessários para estabelecer a troca de dados entre sistemas externos com o Gensoft.

Este protocolo está disponível para clientes externos que desejam se integrar com Gensoft. Portanto, pode ser necessário ter uma implementação externa usando estas bibliotecas aqui descritas para integração.

Usamos como protocolo de comunicação o padrão JSON RESTful APIs, para envio e recebimento de dados.

# Autenticação

Para envio de dados é necessário que o usuário tenha uma API key. Gensoft usa chaves para acesso à API. Para ter uma API key, é necessário falar com nosso [time técnico da Genomika](mailto:suporte@genomika.com.br).

O Gensoft necessita que o API key seja inclusa em todas requisições de API ao servidor dentro do cabeçalho (header) conforme o exemplo abaixo:

`Authorization: meowmeowmeow`


```shell
# Via shell, você pode passar o header correto para cada requisição
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

<aside class="notice">
Não esqueça de substituir o  <code>meowmeowmeow</code> com a API Key fornecida a você.
</aside>


# Passagem ou Pedido

## Envio de Dados


```shell
curl "http://our_url/api/v1/attendance/"
  -H "Authorization: meowmeowmeow"
```

> O comando acima retorna o seguinte JSON estruturado:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

Este endpoint permite o envio para o Gensoft os dados do atendimento cadastrados, junto com os dados do paciente, dados do procedimentos, de amostras requisitadas, solicitantes e arquivos em anexo e dados clínicos.


### HTTP Request

`POST http://our_url/api/v1/attendance/`

### Headers

`Content-Type: application/json`

`Authorization: ApiKey`

### Parâmetros da Requisição

Deverá ser enviado como corpo da requisição 

Parâmetro | Descrição
--------- | -----------
ID | O ID da execução que deseja realizar o upload do laudo em formato PDF.

### Momento de Execução

Este evento deverá ser chamados nos seguintes estágios:

- Quando um novo atendimento é criado pela primeira vez.
- Quando houver cancelamento do atendimento completo.
- Quando houver bloqueio do atendimento completo.
- Quando houver atualização dos anexos e dados clínicos.
- Quando houver atualização de exames (exclusão e/ou adição)

<aside class="success">
Será criado um novo atendimento no Gensoft ou se for no caso de atualização ou cancelamento a operação desejada será realizada no Gensoft. Recomenda-se que o envio seja feito logo após que o atendimento não tenha pendências financeiras, ou seja seja de fato um atendimento criado com sucesso.
</aside>


# Laudos

## Realizar Upload de Laudo


```shell
curl "http://our_url/api/v1/results/upload/execution/10"
  -H "Authorization: meowmeowmeow"
```

> O comando acima retorna o seguinte JSON estruturado:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

Este endpoint permite o upload do laudo em formato PDF para o resultado da execução especificada em parâmetro. Execução no Gensoft, significa a instância do exame daquele paciente em execução no laboratório.

### HTTP Request

`PUT http://our_url/api/v1/results/upload/execution/`

### Parâmetros da Requisição

Parâmetro | Descrição
--------- | -----------
ID | O ID da execução que deseja realizar o upload do laudo em formato PDF.

### Momento de Execução

Este evento deverá ser chamado no momento em que um laudo é liberado no LMS de liberação de laudos. Assim que chamado será enviado um PDF do laudo liberado a execução do exame identificado. Caso haja já um laudo, ele irá substituir o anterior, colocando este como nova versão mais atualizada.

<aside class="success">
O laudo atualizado terá como nome no Gensoft o seguinte rótulo:  xxxx
</aside>
