# 🔓 RemoverSenhaPDF - AWS Lambda 📄

Esta função no AWS Lambda permite remover senhas de arquivos PDF criptografados de forma rápida e segura. O serviço recebe um arquivo PDF codificado em base64 e sua senha, retornando o mesmo PDF livre de proteção.

## ✨ O Que o Código Faz

### 🔧 Funcionalidades Principais

* **Processamento de PDFs**: Recebe arquivos PDF protegidos por senha e remove a proteção
* **Validações de Segurança**: Verifica tamanho do arquivo e formato correto
* **Tratamento de Arquivos Temporários**: Gerenciamento seguro de arquivos durante o processamento
* **Logs Estruturados**: Logs detalhados em formato JSON para fácil monitoramento
* **Tratamento de Erros**: Respostas claras e detalhadas para diferentes cenários de erro
* **Compatibilidade com API Gateway**: Formatação de respostas para integração com AWS API Gateway

## 🚀 Como Utilizar

A função aceita solicitações POST com um corpo JSON contendo:

```json
{
  "pdfBase64": "base64EncodedPdfString",
  "password": "senhaDoArquivo"
}
```

### 📊 Códigos de Resposta

| Código | Situação | Descrição |
|--------|----------|-----------|
| **200** | ✅ Sucesso | Senha removida com êxito, retorna o PDF sem proteção |
| **400** | ❌ Erro | Parâmetros ausentes ou inválidos, método HTTP incorreto |
| **401** | 🔑 Erro | Senha fornecida está incorreta |
| **413** | 📦 Erro | O arquivo excede o tamanho máximo permitido |
| **500** | 💥 Erro | Erro interno durante o processamento do arquivo |

### 🧰 Estrutura da Resposta

```json
{
  "message": "Mensagem de status",
  "pdfBase64": "ArquivoPDFSemSenhaEmBase64" // apenas em caso de sucesso
}
```

Em caso de erro:

```json
{
  "error": "CÓDIGO_DO_ERRO",
  "message": "Descrição detalhada do erro",
  "details": "Detalhes técnicos (apenas para erros internos)"
}
```

## ⚙️ Configuração

A função pode ser configurada através de variáveis de ambiente:

| Variável | Descrição | Valor padrão |
|----------|-----------|--------------|
| `MAX_PDF_SIZE_MB` | Tamanho máximo do PDF em MB | 3.0 |
| `CORS_ORIGIN` | Origem permitida para solicitações CORS | * |

## 📊 Monitoramento

A função implementa logs estruturados em formato JSON, incluindo:
* Identificador de requisição único (UUID)
* Métricas de tempo de processamento
* Detalhes dos arquivos processados
* Erros detalhados com stacktrace

### 📝 Exemplo de Log

```json
{
  "message": "Processamento concluído com sucesso",
  "request_id": "550e8400-e29b-41d4-a716-446655440000",
  "process_time_seconds": 1.234
}
```

## 🛠️ Desenvolvimento e Implantação

### Dependências
- Python 3.8+
- pikepdf

### Implantação
1. Empacote o código e suas dependências (recomendável utilizar um camada/layer com as dependências para melhor modularidade)
2. Implante na AWS Lambda
3. Configure a memória recomendada (128MB, no mínimo)
4. Defina o timeout adequado (10 segundos, no mínimo)
5. Configure um trigger do API Gateway

## 🔒 Segurança

- Os arquivos são processados temporariamente e eliminados após o uso
- Não há armazenamento permanente de dados
- Todo processamento ocorre na memória da função Lambda
- Logs estruturados para auditoria e monitoramento