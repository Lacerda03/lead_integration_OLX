# Documentação para a Integração de Leads com a OLX

Este documento tem como propósito ajudar CRMs/Sistemas/Plataformas/Softwares do mercado Imobiliário e Automotivo a serem integrados com a OLX, para que eles recebam automaticamente os leads gerados na OLX. O sistema de geração de leads **não** contempla o chat da OLX ou leads de telefone/ligação, sendo então restrito apenas aos chamados 'leads de email'.

Um lead é uma solicitação de contato, feita por um comprador no portal OLX. O lead então é entregue ao anunciante com informações básicas para que o contato seja feita, como nome, email, telefone, etc.

### Como integrar com a OLX?

Se você ainda não está integrado com a OLX para recebimento de leads, deverá disponibilizar um endpoint para a OLX homologar a integração de leads.

A OLX requer que cada anunciante tenha um endpoint único. Recomendamos estruturas similares a essas:

* https://seudominio.com.br/olx/lead/TOKEN
* https://TOKEN.seudominio.com.br/olx/lead

O `TOKEN`, nestes exemplos, é o identificador do anunciante na base do sistema ou crm que irá receber o lead. Pode ser utilizado para identificar um determinado cliente da base.

Recomendamos essa estrutura especificamente para sistemas integrados que serão usados por mais do que um anunciante (isso normalmente acontece quando um sistema de mercado é contratado por diversos anunciantes ou quando um sistema é usado por um anunciante que tem filiais e quer manter controle desse contexto.

### Envio de leads

Os dados dos leads serão enviados via protocolo HTTP ou HTTPS com o verbo POST com payload em formato JSON para o endpoint especificado para o cliente.

Cada lead será enviado de forma individual a medida que forem gerados na plataforma OLX.
  
### Status code

Nosso controle de entrega de leads será feito com base no status code do protocolo HTTP: 
* **2XX**: Indica que o lead foi recebido com sucesso.
* **3XX, 4XX ou 5XX**: Indica que houve erro no recebimento do lead.

A OLX guardará essa resposta da entrega do lead para eventual *trouleshoot*. A priori, a OLX não tem política de reenvio ou reprocessamento de leads que não forem recebidos.

É recomendável que seja enviada um `responseId`, para identificar o recebimento do lead e a resposta devolvida referente a esse lead recebido.


### Payload

O JSON enviado na integração de leads tem os seguintes campos:
* `source`: Origem do lead. No caso sempre será OLX. Mas serve para fazer a correta atribuição do lead.
* `adId`: Código identificador do anúncio na base do cliente
* `name`: Nome do cliente que entrou em contato
* `email`: Email do cliente que entrou em contato
* `phone`: Telefone do cliente que entrou em contato (campo opcional)
* `message`: Mensagem enviada pelo cliente
* `createdAt`: Data de criação do lead

Segue exemplo de um JSON para um lead enviado:
```
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

### Timeout
As requisições para o endpoint estão configuradas com timeout de 5 segundos. Caso a requisição que demore mais que 5 segundos será considerada ERRO.


## Homologação, dúvidas e resolução de problemas

Assim que você realizar a construção do endpoint e estiver pronto para homologar, entre em contato com renato.cairo@olxbr.com para realizarmos os testes iniciais.

Caso tenham alguma dúvida ou queiram resolver problema, entrem em contato com renato.cairo@olxbr.com ou suporteintegrador@olxbr.com.
