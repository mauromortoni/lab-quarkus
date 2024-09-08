# Documento de Decisão Arquitetural (ADR)

## Título: Decisão Arquitetural para o Projeto Election Management

### Data: 2024-09-06

---

### 1. Contexto

Este documento descreve as decisões arquiteturais tomadas para o desenvolvimento do Projeto Election Management, que visa criar uma aplicação moderna, escalável e de fácil manutenção. Neste projeto, optamos por uma arquitetura baseada em microserviços, utilizando tecnologias que garantem desempenho, testabilidade e observabilidade, com foco na abordagem de **detecção de fraude** e **auditoria** para o projeto.

### 2. Decisões

#### 2.1 Framework
- **Decisão**: Utilizar **Quarkus** como framework.
- **Justificativa**: Quarkus é otimizado para contêineres e microserviços, oferecendo inicialização rápida e baixo consumo de memória. É projetado para funcionar com o ecossistema Java e se integra bem com outras tecnologias que utilizamos.
- **Referências**:
    - [Quarkus Documentation](https://quarkus.io/guides/)
    - [Quarkus: Supersonic Subatomic Java](https://quarkus.io/)

#### 2.2 Linguagem de Desenvolvimento
- **Decisão**: Utilizar **Java** como linguagem de desenvolvimento.
- **Justificativa**: Java é uma linguagem madura com uma vasta comunidade e suporte robusto. Sua compatibilidade com Quarkus e outras tecnologias do stack escolhido justifica sua utilização.
- **Referências**:
    - [Java SE Documentation](https://docs.oracle.com/en/java/javase/11/docs/api/index.html)
    - [Java Community](https://www.oracle.com/java/technologies/javase/community.html)

#### 2.3 Testabilidade
- **Decisão**: Utilizar **JUnit** para testes.
- **Justificativa**: JUnit é uma das bibliotecas de teste mais populares para Java, oferecendo recursos robustos para testes unitários e de integração, facilitando a verificação da funcionalidade do código.
- **Referências**:
    - [JUnit 5 User Guide](https://junit.org/junit5/docs/current/user-guide/)
    - [Testing in Java with JUnit](https://www.baeldung.com/junit-5)

#### 2.4 Mapeamento Objeto-Relacional
- **Decisão**: Utilizar **Hibernate** como solução de mapeamento objeto-relacional (ORM).
- **Justificativa**: Hibernate é amplamente utilizado na comunidade Java e oferece suporte a uma variedade de bancos de dados, além de facilitar a manipulação de dados de forma transparente.
- **Referências**:
    - [Hibernate ORM Documentation](https://hibernate.org/orm/documentation/)
    - [Getting Started with Hibernate](https://www.baeldung.com/hibernate-5)

#### 2.5 Migração
- **Decisão**: Utilizar **Flyway** para migrações de banco de dados.
- **Justificativa**: Flyway permite uma gestão de versão de esquemas de banco de dados de forma simples e eficaz, garantindo que as alterações sejam aplicadas de maneira controlada.
- **Referências**:
    - [Flyway Documentation](https://flywaydb.org/documentation/)
    - [Getting Started with Flyway](https://flywaydb.org/getstarted/)

#### 2.6 Observabilidade
- **Decisão**: Utilizar uma combinação de **Graylog**, **OpenTelemetry**, **Jaeger** e **Traefik** para observabilidade.
- **Justificativa**:
    - **Graylog** para gerenciamento de logs centralizados.
    - **OpenTelemetry** para coleta de métricas e traços.
    - **Jaeger** para rastreamento distribuído, permitindo a identificação de gargalos de desempenho.
    - **Traefik** como proxy reverso, facilitando o gerenciamento de tráfego e a configuração dinâmica.
- **Referências**:
    - [Graylog Documentation](https://docs.graylog.org/en/latest/)
    - [OpenTelemetry Documentation](https://opentelemetry.io/docs/)
    - [Jaeger Documentation](https://www.jaegertracing.io/docs/)
    - [Traefik Documentation](https://doc.traefik.io/traefik/)

#### 2.7 Conteinerização
- **Decisão**: Utilizar **Kubernetes** para orquestração de contêineres.
- **Justificativa**: Kubernetes é a solução de orquestração de contêineres mais popular, oferecendo escalabilidade, alta disponibilidade e gerenciamento eficiente de recursos.
- **Referências**:
    - [Kubernetes Documentation](https://kubernetes.io/docs/home/)
    - [Kubernetes Basics](https://kubernetes.io/docs/tutorials/kubernetes-basics/)

#### 2.8 Armazenamento
- **Decisão**: Utilizar **MariaDB** e **Redis** para armazenamento de dados.
- **Justificativa**:
    - **MariaDB** como banco de dados relacional, oferecendo robustez e compatibilidade com MySQL.
    - **Redis** como armazenamento em memória para cache, aumentando a performance e reduzindo a latência nas operações de leitura.
- **Referências**:
    - [MariaDB Documentation](https://mariadb.com/kb/en/documentation/)
    - [Redis Documentation](https://redis.io/documentation)

**Estrutura de Dados**
- A estrutura de dados está na **Terceira Forma Normal (3NF)**.
- A implementação de chaves primárias e restrições exclusivas assegura a integridade dos dados, prevenindo valores duplicados nas colunas definidas.
- A auditoria que valoriza a integridade, a transparência e a responsabilidade, que estabelece um sistema robusto que não apenas ajuda a garantir a conformidade, mas também melhora a eficiência operacional e a segurança.

- **Registro de Eleitores**: Manter um banco de dados atualizado com informações únicas de cada eleitor.

**Entidade Elector**

| PK | Id            |
|----|---------------|
|    | Nome          |
|    | Data Nascimento |
|    | ...           |

- **Verificação de Identidade**: Implementar autenticação robusta (ex.: biometria, autenticação multifatorial) para confirmar a identidade do eleitor antes de permitir a votação.

- **Registro de Eleições**: Manter um banco de dados atualizado com informações únicas de cada eleição.

**Entidade Election**

| PK | Id            |
|----|---------------|
|    | ...           |


- **Registro de Candidatos**: Manter um banco de dados atualizado com informações únicas de cada candidato.

**Entidade Candidate**

| PK | Id            |
|----|---------------|
|    | Nome          |
|    | ...           |


- **Registro de Votos**: Manter um banco de dados atualizado com informações únicas de cada voto.

**Entidade Vote**

| PK  | electionId   |
|-----|--------------|
| PK  | electorId    |
|     | candidateId  |
|     | date_vote    |


### 3. Consequências

#### Positivas

A adoção dessas tecnologias e práticas proporcionará:

- **Desempenho elevado**: Graças à combinação de Quarkus, Kubernetes e Redis.
- **Escalabilidade**: A arquitetura baseada em microserviços permite que os componentes sejam escalados independentemente.
- **Facilidade de manutenção**: O uso de práticas de teste e migração de banco de dados simplifica a evolução do software.
- **Visibilidade**: A integração de ferramentas de observabilidade permitirá uma melhor detecção de problemas e análise de desempenho.
- **Unicidade do Voto**: A combinação de `electionId` e `electorId` na entidade Vote garante que cada eleitor tenha um único voto por eleição.
- **Segurança**: A autenticação do eleitor fortalece a segurança do processo de votação.
- **Prevenção de Votos Múltiplos**: O sistema impede que um eleitor vote mais de uma vez na mesma eleição.
- **Auditoria e Melhoria Contínua**: A criação da entidade Vote permite a auditoria das eleições, facilitando a análise de falhas e a tomada de decisões informadas para melhorias futuras.

#### Negativas

- **Sobrecarga de Processamento**: A criação das novas entidades e chaves primárias adiciona complexidade e processamento adicional à aplicação.

- **Desempenho**
- Overhead em Operações: A verificação de integridade referencial pode adicionar latência a operações de inserção, atualização e exclusão, especialmente em tabelas grandes ou em transações com múltiplas operações.
- Bloqueios: As chaves estrangeiras podem causar bloqueios em tabelas relacionadas, o que pode impactar o desempenho em ambientes de alta concorrência.

- **Complexidade da Manutenção**
- Alterações de Schema: A modificação da estrutura do banco de dados pode se tornar mais complexa quando chaves estrangeiras estão em uso, pois podem ser necessárias alterações em várias tabelas relacionadas.
- Cascading Operations: Configurações de exclusão em cascata ou atualização em cascata podem levar a comportamentos inesperados se não forem cuidadosamente gerenciadas.


### 4. Próximos Passos

1. **Configuração do ambiente de desenvolvimento** com as tecnologias escolhidas.
2. **Implementação das primeiras funcionalidades** utilizando as decisões arquiteturais definidas.
3. **Desenvolvimento de um plano de testes** utilizando JUnit.
4. **Estabelecimento de protocolos de migração** utilizando Flyway.

---

## Assinado por:
- **Autor**: Mauro Mortoni
- **Versão**: 0.1
- **Registro de alterações**:
    - 0.1: Versão inicial proposta
- Data: 2024-09-06
---

**Observação**:
Este documento serve como um registro das decisões arquiteturais e deve ser revisado e atualizado conforme o projeto evolui.
