# Objetivo
Compreender seu uso.

---
O **Get-WinEvent** é um cmdlet do PowerShell utilizado para consultar Windows Event Logs e arquivos de Event Tracing, tanto localmente quanto em computadores remotos. Ele permite combinar eventos de diferentes fontes e realizar filtros usando XPath, XML estruturado e FilterHashtable.

Em logs grandes é recomendado usar **-FilterHashtable**, que realiza um filtro diretamente na consulta.

O Get-WinEvent é especialmente útil em SOC e investigação de incidentes, pois permite automatizar consultas, filtrar grandes volumes de eventos e criar scripts para localizar indicadores relevantes com mais eficiência do que a análise manual pelo Event Viewer.

---
# Desafios do lab
1. 
