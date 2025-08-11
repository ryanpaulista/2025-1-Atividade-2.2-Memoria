# S.O. 2025.1 - Atividade 2.02 - Gestão de memória

## Informações gerais

- **Objetivo do repositório**: Repositório para atividade avaliativa dos alunos
- **Assunto**: Gestão de memória
- **Público alvo**: alunos da disciplina de SO (Sistemas Operacionais) do curso de TADS (Superior em Tecnologia em Análise e Desenvolvimento de Sistemas) no CNAT-IFRN (Instituto Federal de Educação, Ciência e Tecnologia do Rio Grande do Norte - Campus Natal-Central).
- disciplina: **SO** [Sistemas Operacionais](https://github.com/sistemas-operacionais/)
- professor: [Leonardo A. Minora](https://github.com/leonardo-minora)
- Repositótio do aluno: FIXME

## Tarefas do aluno
1. Fork desse repositório e atualizar a linha 10 com o nome e link do github
2. Ler a descrição da atividade
3. Montar a resposta no final deste arqivo, no tópico **Resposta**

---

## 1. Descrição da atividade
### 1.1. Objetivo
Praticar os conceitos de alocação de memória (best-fit), memória virtual e desfragmentação em um sistema com memória limitada.

---

### 1.2. Contexto
Um computador possui apenas **64 KB de RAM** e um **disco rígido para memória virtual**. O sistema operacional deve gerenciar 5 processos com tamanhos diferentes, cuja soma ultrapassa a capacidade da RAM.

#### 1.2.1. Processos iniciais

| Processo | Tamanho (KB) |
|----------|-------------|
| P1       | 20          |
| P2       | 15          |
| P3       | 25          |
| P4       | 10          |
| P5       | 18          |
| **Total**| **88 KB**   |

- **Memória RAM**: 64 KB (contígua, inicialmente vazia).  
- **Memória Virtual (Disco)**: Espaço ilimitado para paginação.

#### 1.2.2. Alocação Inicial com Best-Fit
Os alunos devem simular a alocação dos processos na RAM usando o algoritmo **best-fit**.  
- A memória RAM será representada como um bloco contíguo (ex: `[0KB - 64KB]`).  
- Devem alocar os processos nos menores espaços livres que atendam ao seu tamanho.  

**Alocação inicial**:  
1. P1 (20 KB) → Ocupa [0-20].  
2. P2 (15 KB) → Ocupa [20-15].  
3. _continuar a partir daqui_

#### 1.2.3. Simular Memória Virtual (Paginação)
- Os processos não alocados na RAM devem ser "paginados" no disco.  
- Criar uma tabela de páginas indicando quais partes estão na RAM e quais estão no disco.  

#### 1.2.4. Desfragmentação da RAM
- Desfragmentar a RAM para liberar espaço contíguo.
- Após desfragmentação (compactação), verificar quais processos podem ser alocado.  

### 1.3. Questões para Reflexão
1. Best-fit foi mais eficiente que first-fit ou worst-fit neste cenário?  
2. Como a memória virtual evitou um deadlock?  
3. Qual o impacto da desfragmentação no desempenho do sistema?  

---

## Resposta

# Simulação de Gerenciamento de Memória: Best-Fit, Memória Virtual e Desfragmentação

## 1. Cenário

* **Memória RAM:** 64 KB (um bloco contíguo)
* **Memória Virtual:** Espaço em disco, considerado ilimitado.
* **Algoritmo de Alocação:** Best-Fit
* **Processos a serem gerenciados:**
    | Processo | Tamanho (KB) |
    | :------- | :----------- |
    | P1       | 20           |
    | P2       | 15           |
    | P3       | 25           |
    | P4       | 10           |
    | P5       | 18           |
    | **Total** | **88 KB** |

---

## 2. Alocação Inicial com Best-Fit

O algoritmo **Best-Fit** aloca um processo no menor espaço de memória livre (buraco) que seja grande o suficiente para contê-lo.

**Estado Inicial:**
* RAM: `[ Free (64 KB) ]`

**Passo a Passo da Alocação:**

1.  **Alocar P1 (20 KB):**
    * Só há um buraco de 64 KB. P1 é alocado nele.
    * **RAM:** `[ P1 (20 KB) | Free (44 KB) ]`
    * *Endereços: P1 ocupa `[0 - 19]`.*

2.  **Alocar P2 (15 KB):**
    * Só há um buraco de 44 KB. P2 é alocado nele.
    * **RAM:** `[ P1 (20 KB) | P2 (15 KB) | Free (29 KB) ]`
    * *Endereços: P2 ocupa `[20 - 34]`.*

3.  **Alocar P3 (25 KB):**
    * Só há um buraco de 29 KB. P3 é alocado nele.
    * **RAM:** `[ P1 (20 KB) | P2 (15 KB) | P3 (25 KB) | Free (4 KB) ]`
    * *Endereços: P3 ocupa `[35 - 59]`.*

4.  **Alocar P4 (10 KB):**
    * O único buraco disponível é de 4 KB, que é insuficiente para P4.
    * **P4 não pode ser alocado na RAM.**

5.  **Alocar P5 (18 KB):**
    * O único buraco disponível é de 4 KB, que é insuficiente para P5.
    * **P5 não pode ser alocado na RAM.**

**Resultado da Alocação Inicial:**
* **RAM Ocupada:** 60 KB (P1, P2, P3)
* **RAM Livre:** 4 KB (fragmento no final da memória)
* **Mapa Final da RAM:** `[ P1 (20) | P2 (15) | P3 (25) | Free (4) ]`

---

## 3. Simulação de Memória Virtual (Paginação)

Os processos que não puderam ser alocados na RAM (P4 e P5) são movidos para a memória virtual no disco. O sistema operacional mantém uma tabela para rastrear a localização de cada processo.

**Tabela de Status dos Processos:**

| Processo | Tamanho (KB) | Status          | Localização       |
| :------- | :----------- | :-------------- | :---------------- |
| P1       | 20           | Na RAM          | Endereços `[0-19]`  |
| P2       | 15           | Na RAM          | Endereços `[20-34]` |
| P3       | 25           | Na RAM          | Endereços `[35-59]` |
| **P4** | **10** | **Em Disco** | **Memória Virtual** |
| **P5** | **18** | **Em Disco** | **Memória Virtual** |

---

## 4. Desfragmentação da RAM

Para demonstrar a desfragmentação, vamos simular a **terminação do processo P2**, que está entre P1 e P3, criando um buraco no meio da memória (fragmentação externa).

**Passo 1: Terminação do Processo P2**

* O processo P2 (15 KB) libera seu espaço.
* **Mapa da RAM (Fragmentado):** `[ P1 (20 KB) | Free (15 KB) | P3 (25 KB) | Free (4 KB) ]`
* **Memória Livre Total:** 15 KB + 4 KB = 19 KB.
* **Problema:** Embora tenhamos 19 KB livres, o maior bloco *contíguo* é de apenas 15 KB. Portanto, o processo **P5 (18 KB)**, que está no disco, ainda não pode ser carregado.

**Passo 2: Executando a Desfragmentação (Compactação)**

O sistema operacional move os processos alocados para uma extremidade da memória, unindo todos os buracos livres em um único bloco contíguo.

* O processo P3 é movido para junto do P1.
* **Mapa da RAM (Pós-Compactação):** `[ P1 (20 KB) | P3 (25 KB) | Free (19 KB) ]`

**Passo 3: Alocação Pós-Desfragmentação**

* Agora temos um bloco livre contíguo de 19 KB.
* O sistema operacional verifica os processos em disco e tenta alocá-los.
* O processo **P5 (18 KB)** pode ser alocado no buraco de 19 KB.
* **Mapa Final da RAM:** `[ P1 (20) | P3 (25) | P5 (18) | Free (1) ]`

A desfragmentação permitiu que um processo maior fosse carregado na memória, otimizando o uso do recurso.

---

## 5. Questões para Reflexão

**1. Best-fit foi mais eficiente que first-fit ou worst-fit neste cenário?**

* **Neste cenário inicial específico**, não houve diferença. Tanto o **First-Fit** quanto o **Worst-Fit** teriam produzido o mesmo resultado, pois a cada passo havia apenas um único buraco para alocar.
* **Teoricamente:**
    * **Best-Fit** tende a preservar grandes buracos, mas pode criar pequenos fragmentos inúteis (como o de 4 KB), um fenômeno conhecido como "fragmentação externa".
    * **First-Fit** é mais rápido, mas pode quebrar grandes blocos de memória desnecessariamente.
    * **Worst-Fit** aloca no maior buraco, deixando um buraco restante que ainda é grande e potencialmente útil. No entanto, ele elimina rapidamente os maiores buracos, o que pode ser um problema para processos grandes que cheguem depois.
* **Conclusão:** A eficiência de cada algoritmo depende muito da sequência e do tamanho dos processos que chegam e saem da memória. Best-Fit foi eficiente em "empacotar" os processos, mas a um custo de criar um fragmento minúsculo.

**2. Como a memória virtual evitou um deadlock?**

Tecnicamente, o cenário apresentado não é um *deadlock* (processos travados esperando um pelo outro), mas sim uma **insuficiência de recursos** (RAM).
A memória virtual resolveu essa insuficiência da seguinte forma:

* **Estendendo o Espaço de Endereçamento:** A memória virtual permitiu que o sistema operacional "prometesse" memória aos processos P4 e P5, mesmo sem ter RAM física disponível. Ela usou o disco como uma extensão da RAM.
* **Evitando a Rejeição:** Sem a memória virtual, o sistema operacional teria que rejeitar P4 e P5, impedindo sua execução. Com ela, os processos foram colocados em uma "fila de espera" no disco.
* **Permitindo a Execução:** Isso garante que o sistema continue produtivo. Quando um espaço na RAM é liberado (seja por término de processo ou swapping), um processo do disco pode ser carregado, garantindo o progresso e o compartilhamento de recursos ao longo do tempo.

**3. Qual o impacto da desfragmentação no desempenho do sistema?**

A desfragmentação tem um impacto duplo:

* **Impacto Positivo (Benefício):** Aumenta a **utilização da memória**. Como visto na simulação, a compactação permitiu que 19 KB de memória livre fragmentada se tornassem um bloco único utilizável, possibilitando a execução do processo P5. Isso melhora o throughput do sistema a longo prazo.

* **Impacto Negativo (Custo):** A desfragmentação é uma operação extremamente **custosa em termos de CPU**. O sistema precisa:
    1.  **Parar a execução** dos processos que serão movidos.
    2.  **Copiar** grandes blocos de dados de uma área da memória para outra, o que consome muitos ciclos de CPU.
    3.  **Atualizar** as referências de memória do sistema operacional (como registradores de base e limite) para apontar para os novos endereços dos processos movidos.
* **Consequência:** Durante a compactação, o sistema fica "congelado" e não responsivo ao usuário. Por esse motivo, ela é usada com pouca frequência em sistemas de tempo real ou interativos, sendo mais comum em sistemas de processamento em lote (batch).
