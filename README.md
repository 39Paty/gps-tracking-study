# 📍 Sistema de Rastreamento GPS Infantil

## 1. Contexto e Objetivos
Este projeto apresenta um estudo técnico sobre a viabilidade de dispositivos de monitoramento GPS para segurança familiar, explorando a integração entre hardware, back-end e privacidade.

## 2. Curadoria de Fontes
As informações foram extraídas e analisadas via NotebookLM com base no documento:
* `estudo_tecnico_gps.pdf` (disponível na pasta deste repositório).

## 3. Engenharia de Prompts e Insights da IA
Durante o estudo no NotebookLM, utilizei prompts estratégicos para entender a complexidade do sistema:

* **Pergunta feita à IA:** "Quais os principais desafios de hardware?"
* **Resposta obtida:** Com base nos documentos fornecidos e em nossa conversa anterior, os principais desafios de hardware para o sistema de rastreamento (utilizando o chip NEO-6M e o microcontrolador ESP32) são:
Eficiência Energética (Consumo de Bateria): Este é o desafio mais crítico, visto que o sistema é voltado para dispositivos wearables (vestíveis) para segurança infantil
. A escolha da arquitetura de hardware e dos protocolos de transmissão é feita especificamente para economizar bateria, preferindo o uso de MQTT em vez de HTTP puro para garantir a autonomia do dispositivo
.
Comunicação entre Componentes: O módulo GPS NEO-6M precisa se comunicar com o microcontrolador (como o ESP32) através de uma interface UART (Serial)
. Garantir a estabilidade dessa comunicação física e a correta recepção dos dados é um requisito técnico essencial do hardware.
Complexidade do Dado Bruto (NMEA 0183): O hardware gera informações no formato de sentenças GPRMC, que são strings de dados brutos complexas (contendo latitude, longitude, velocidade, data, etc.)
. O desafio reside em transmitir esses dados de forma íntegra para que o back-end possa validar o checksum e processar a localização corretamente
.
Portabilidade e Aplicação: Por ser um sistema destinado a dispositivos móveis e segurança infantil, o hardware deve ser compacto o suficiente para ser "vestível", o que limita o tamanho da bateria e exige uma gestão de energia extremamente eficiente por parte dos componentes eletrônicos

* **Dificuldade encontrada:** Inicialmente, tive dúvida sobre como o dado chegava no servidor, mas a IA esclareceu o uso do protocolo NMEA.

## 4. Miniguia de Estudo (Consolidado)
* **Resumo:** Este Miniguia de Estudo consolida as informações técnicas e regulatórias sobre o sistema de rastreamento GPS para segurança infantil, com base nas fontes fornecidas:
* . Hardware e Captação de Dados
Componente Principal: O sistema utiliza o módulo GPS NEO-6M, que opera sob o protocolo NMEA 0183
.
Comunicação Física: O módulo se conecta a microcontroladores (como o ESP32) através de uma interface UART (Serial)
.
O Dado GPRMC: Esta é a sentença de dado bruto fundamental, pois concentra as informações de latitude, longitude e velocidade necessárias para o rastreamento
.
2. Arquitetura e Transmissão (Back-end)
Eficiência Energética: Para dispositivos wearables (vestíveis), o uso do protocolo MQTT é preferível ao HTTP puro para economizar bateria
.
Processamento no Servidor: Utilizando frameworks como Python/Django, o servidor deve receber os dados (geralmente via JSON), validar o checksum da sentença GPRMC e armazenar as informações em um banco de dados relacional PostgreSQL para auditoria
.
Cerca Virtual (Geofencing): O sistema deve ser capaz de identificar se o dispositivo saiu de um raio geográfico pré-definido e, em caso positivo, disparar alertas em tempo real via Firebase Cloud Messaging
.
3. Segurança e Conformidade (LGPD)
Tratamento de Dados Sensíveis: A localização de menores é classificada como dado sensível, exigindo proteção rigorosa
.
Criptografia: É mandatório o uso de TLS para dados em trânsito e AES-256 para dados em repouso (armazenados)
.
Retenção de Dados: Seguindo o princípio da minimização, os logs de movimentação devem possuir uma rotina de exclusão automática após 30 dias
.

### 📖 Glossário Técnico
1. **NMEA:** Protocolo de comunicação padrão para recepção de dados de satélite.
2. **JSON:** Formato leve de troca de dados, ideal para integração entre o chip e o Back-end.
3. **LGPD:** Lei Geral de Proteção de Dados, fundamental para garantir a privacidade no rastreamento de menores.
