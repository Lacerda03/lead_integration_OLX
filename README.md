# Documentação para a Integração de Leads com a OLX

Este documento tem como propósito ajudar CRMs/Sistemas/Plataformas/Softwares do mercado Imobiliário e Automotivo a serem integrados com a OLX, para que eles recebam automaticamente os leads gerados na OLX. Um lead é uma solicitação de contato, feita por um comprador no portal OLX. O lead então é entregue ao anunciante com informações básicas para que o contato seja feita, como nome, email, telefone, etc.

**Importante**: O sistema de integração de leads está restrito aos chamados "leads de emails" e só funciona para anunciantes que configurarem suas contas para receberem lead de email ao invés de chats. Não há como o anunciante receber um lead de email via integração e continuar com o chat. Para receber o lead de email ele precisa configurar sua conta para essa modalidade de contato.

### Como integrar com a OLX?

Se você ainda não está integrado com a OLX para recebimento de leads, deverá disponibilizar um endpoint para a OLX homologar a integração de leads.

A OLX requer que cada anunciante tenha um endpoint único. Recomendamos estruturas similares a essas:

* https://seudominio.com.br/olx/lead/TOKEN
* https://TOKEN.seudominio.com.br/olx/lead

O `TOKEN`, nestes exemplos, é o identificador do anunciante na base do sistema ou CRM que receberá o lead. Pode ser utilizado para identificar um determinado cliente da base.

Recomendamos essa estrutura especificamente para sistemas integrados que serão usados por mais do que um anunciante (isso normalmente acontece quando um sistema de mercado é contratado por diversos anunciantes ou quando um sistema é usado por um anunciante que tem filiais e quer manter controle desse contexto).


### Envio de leads

Os dados dos leads serão enviados via protocolo HTTP ou HTTPS com o verbo POST com payload em formato JSON para o endpoint especificado para o cliente.

Cada lead será enviado de forma individual a medida que forem gerados na plataforma OLX, contendo os seguintes campos:

| Parâmetro | Obrigatório | Descrição |
|-------------|-------------|---------------------------------------------------------------------------------------------------------------|
| `source` | Sim | Origem do lead. Serve para fazer a correta atribuição do lead. Sempre enviaremos o valor `OLX`. |
| `adId` | Sim | É o identificador do anúncio, para que o anunciante saiba o anúncio relacionado a esse lead. |
| `name` | Sim | Nome do cliente que entrou em contato. |
| `email` | Sim | Email do cliente que entrou em contato. |
| `phone` | Não | Telefone do cliente que entrou em contato. Sequência numérica de até 13 caracteres. Telefones podem vir com ou sem DDD. |
| `message` | Sim | Mensagem enviada pelo cliente. |
| `createdAt` | Sim | Data e hora da geração do lead. |

Segue exemplo de um JSON para um lead enviado:

```json
{
  "source": "OLX",
  "adId": "a1234",
  "name": "Nome do cliente",
  "email": "email.docliente@gmail.com",
  "phone": "21 9999-9999",
  "message": "Olá, gostaria de saber mais informações sobre o anúncio a1234", 
  "createdAt": "2019-02-12T14:30:00.500Z"
}
```

### Status code

Nosso controle de entrega de leads será feito com base no status code do protocolo HTTP: 
* `2XX`: Indica que o lead foi recebido com sucesso.
* `3XX`, `4XX` ou `5XX`: Indica que houve erro no recebimento do lead.

A OLX guardará essa resposta da entrega do lead para eventual *troubleshoot*. A priori, a OLX não tem política de reenvio ou reprocessamento de leads que não forem recebidos.

É recomendável que seja enviada um `responseId`, para identificar o recebimento do lead e a resposta devolvida referente a esse lead recebido.


### Timeout
As requisições para o endpoint estão configuradas com timeout de 5 segundos. Caso a requisição que demore mais que 5 segundos será considerada ERRO.


## Homologação, dúvidas e resolução de problemas

Para a homologação, entre em contato com suporteintegrador@olxbr.com apenas quando você já tiver:

1) Um endpoint para homologação preparado
2) Alinhado com um anunciante OLX que usa seu sistema sobre a homologação (uma vez que ele pode receber leads de teste em sua conta)
3) Confirmado com esse anunciante que ele recebe leads de email (e não chat) na OLX
4) Identificado o CNPJ desse anunciante (que usaremos internamente para identificar o anunciante e realizarmos o cadastro aqui na OLX)

Com esses requisitos satisfeitos, por favor entre em contato com suporteintegrador@olxbr.com e nós realizaremos os testes para a homologação.
