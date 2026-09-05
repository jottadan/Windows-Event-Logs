# Enquanto fazia outra room montei queries que achei interessantes. Vou deixar guardado aqui.
---
## A pergunta: What was the Logon ID of the malicious RDP login? | Note: The login you are looking for has a Logon Type 10.

Minha 1ª montagem dessa query foi | **_Get-WinEvent -Path "C:\Users\Administrator\Desktop\Practice-Security.evtx" -FilterXPath "*/System/EventID='4624' and */EventData/Data[@Name='IpAddress']='10.10.53.248' and */EventData/Data[@Name='LogonType']=10"_**

> O meu objetivo era encontrar qual o Logon ID da conexão RDP feita pelo atacante. A query retornava só um resultado, sem mostrar justamente o campo do Logon ID.

Após um tempo estudando em como puxar esse campo, dentre diversos meios diferentes e mais complexos que pude ver, escolhi o Format-List *.
Também descobri em qual tipo de info dentro da query precisava usar ""/'', sendo só em números não inteiros ou nomes específicos que queria puxar, e que o jeito que escrevia as queries era em um formato _flat_, então na próxima query montei em outro formato, já utilizando o format-list.

Ficando assim | **_Get-WinEvent -Path "C:\Users\Administrator\Desktop\Practice-Security.evtx" -FilterXPath "*[System[EventID=4624] and EventData[Data[@Name='IpAddress']='10.10.53.248'] and EventData[Data[@Name='LogonType']=10]]" | Format-List *_**

> Os outros jeitos de puxar o campo específico, eram em parte mais complexos e demorados de se escrever. Considerei que para usar como um "copia e cola" eles seriam bem úteis, como para servir de template para puxar a mesma info de um campo específico mas de diversos logs diferentes.

> Para essa info de somente um log, considerei desnecessário e mais prático simplesmente puxar o "detail" todo do evento e procurar manualmente o campo *Logon ID*.

## Neste outro formato, enquanto escrevia pude sentir que em algum momento ficaria perdido com a quantidade de [] abertos/fechados, mas uma simples contagem de colchetes no final faz valer a pena o tempo reduzido de pressionar */.

---

## A pergunta: Which URL was the file downloaded from? | Contexto: um usuário fez download de um arquivo suspeito.

Passei um tempo tentando entender como montar a query de uma forma prática pro uso real, no final cheguei em 2 queries com a ajuda do ChatGPT.

- **Get-WinEvent -Path "... -FilterXPath "*[System[EventID=15] and EventData[Data[@Name='Contents']]]" | ForEach-Object {
([xml]$_.ToXml()).Event.EventEata.Data |
Where-Object Name -eq 'Contents' |
Select-Object -ExpandProperty '#text'
}**

> Por eu não conhecer essas sintaxes e me parecer muita coisa pra escrever, não optei muito por ela. Porém ela não requer investigação adicional além do EventID e o campo específico sendo procurado (diferente da outra query).

> A longo prazo é uma query muito útil de se decorar pra utilizar em um ambiente profissional, não tendo que fazer investigações adicionais pra analisar um event só.

- **Get-WinEvent -Path "..." -FilterXPath "*[System[EventID=15] and EventData[Data[@Name='Contents']]]" | ForEach-Object {$_.Properties}** & **Get-WinEvent -Path "..." -FilterXPath "*[System[EventID=15] and EventData[Data[@Name='Contents']]]" | ForEach-Object {$_.Properties[*].Value}**

> Uso muito mais simples e rápido comparado a 1ª query, porém ela requer dois comandos.

> 1º **Get-WinEvent -Path "..." -FilterXPath "*[System[EventID=15] and EventData[Data[@Name='Contents']]]" | ForEach-Object {$_.Properties}** para conseguir fazer a contagem de _linhas_ para achar o valor do "Contents".

> 2º Substituir o {$_.Properties} por {$_.Properties[...].Value} ("..." sendo o valor em nº da linha do "Contents", Ex.: [8]).

> **Eu considero perfeito para uso único, dois comandos rápidos para pegar uma variável que pode acabar mudando seu valor, mas para uso contínuo haveria maior tempo sendo gasto procurando pelo valor da variável**
