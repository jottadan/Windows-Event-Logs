# Objetivo
Compreender o seu uso.

---

XPath (XML Path Language) é utilizado para navegar e filtrar elementos de documentos XML. O Windows Event Log suporta um subconjunto do XPath 1.0, permitindo criar filtros mais específicos para consultas de eventos.

Tanto o Get-WinEvent quanto o wevtutil.exe suportam XPath como mecanismo de filtragem.

Uma consulta XPath começa normalmente com * ou Event e pode seguir a estrutura do XML do evento. O Event Viewer → Details → XML View é útil para identificar os elementos e atributos que podem ser utilizados na construção da consulta.

- Exemplo filtrando pelo Event ID 100:

Get-WinEvent -LogName Application -FilterXPath '*/System/EventID=100'

- Para filtrar pelo Provider Name, é necessário utilizar o atributo Name:

Get-WinEvent -LogName Application -FilterXPath '*/System/Provider[@Name="WLMS"]

- Também é possível filtrar usando os dois parâmetros adicionando **and** após terminar a 1ª filtragem:

...'*/System/EventID=101 **and** */System/Provider[@Name="WLMS"]'

- Elementos dentro de EventData possuem uma sintaxe própria:

...'*/EventData/Data[@Name="TargetUserName"]="System"'

**No contexto de SOC e investigação de incidentes, XPath permite criar consultas mais precisas sobre os eventos, filtrando por Event ID, Provider, atributos e dados específicos do evento, reduzindo significativamente o volume de informações analisadas.**

---
# Desafios do lab
1. *Com base no conhecimento adquirido sobre Get-WinEvent e XPath, qual é a consulta para encontrar eventos do WLMS com um horário de sistema de 2020-12-15T01:09:08.940277500Z?*
> Get-WinEvent -LogName Application -FilterXPath '*/System/Provider[@Name="WLMS"] and */System/TimeCreated[@SystemTime="2020-12-15T01:09:08.940277500Z"]' | Tive que pesquisar em qual log ficava armazenado o WLMS.
![z](imagens/z)

2. 
