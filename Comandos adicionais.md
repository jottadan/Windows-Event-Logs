# Enquanto fazia outra room montei queries que achei interessantes. Vou deixar guardado aqui.
---
## A pergunta: What was the Logon ID of the malicious RDP login? | Note: The login you are looking for has a Logon Type 10.

Minha 1ª montagem dessa query foi | **_Get-WinEvent -Path "C:\Users\Administrator\Desktop\Practice-Security.evtx" -FilterXPath "*/System/EventID='4624' and */EventData/Data[@Name='IpAddress']='10.10.53.248' and */EventData/Data[@Name='LogonType']=10"_**

> O meu objetivo era encontrar qual o Logon ID da conexão RDP feita pelo atacante. A query retornava só um resultado, sem mostrar justamente o campo do Logon ID.

Após um tempo estudando em como puxar esse campo, dentre diversos meios diferentes e mais complexos que pude ver, escolhi o Format-List *.
Também descobri em qual tipo de info dentro da query precisava usar ""/'', sendo só em números não inteiros ou nomes específicos que queria puxar, e que o jeito que escrevia as queries era em um formato _flat_, então na próxima query montei em outro formato, já utilizando o format-list.

Ficando assim | **_Get-WinEvent -Path "C:\Users\Administrator\Desktop\Practice-Security.evtx" -FilterXPath "*[System[EventID=4624] and EventData[Data[@Name='IpAddress']='10.10.53.248'] and EventData[Data[@Name='LogonType']=10]]" | Format-List *_**

Os outros jeitos de puxar o campo específico, eram em parte mais complexos e demorados de se escrever. Considerei que para usar como um "copia e cola" eles seriam bem úteis, como para servir de template para puxar a mesma info de um campo específico mas de diversos logs diferentes.

Para essa info de somente um log, considerei desnecessário e mais prático simplesmente puxar o "detail" todo do evento e procurar manualmente o campo *Logon ID*.

---
## Neste outro formato, enquanto escrevia pude sentir que em algum momento ficaria perdido com a quantidade de [] abertos/fechados, mas uma simples contagem de colchetes no final faz valer a pena o tempo reduzido de pressionar */.
