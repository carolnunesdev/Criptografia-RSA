Com certeza. O material a seguir está formatado em Markdown, pronto para ser copiado e colado em um arquivo de documentação Git (como `README.md`), garantindo que a atividade fique completa e explicativa, conforme solicitado.

---

```markdown
# 🔐 Criptografia RSA: Análise Técnica, Implementação e Segurança

## 📊 Índice Detalhado

1.  Introdução e Fundamentos do RSA
2.  A Matemática por Trás do Algoritmo
3.  O Algoritmo RSA Passo-a-Passo
4.  Detalhes de Implementação Algorítmica
5.  Análise de Segurança e Tamanho das Chaves
6.  Otimizações e Limitações
7.  Conclusões e Uso em Produção

---

## 1. Introdução e Fundamentos do RSA

O **RSA** (Rivest-Shamir-Adleman), desenvolvido em 1977, é um dos sistemas de **criptografia de chave pública** mais antigos e amplamente utilizados.

### 1.1 Características Principais

*   **Assimétrico:** Utiliza um par de chaves distintas: uma **chave pública** (para criptografar e verificar assinaturas) e uma **chave privada** (para descriptografar e assinar).
*   **Base Matemática:** Sua segurança reside na **dificuldade computacional de fatorar números grandes** (Problema da Fatoração).
*   **Bidirecional:** Pode ser usado tanto para a **criptografia de dados** quanto para a criação de **assinaturas digitais**.
*   **Adoção:** É o alicerce de muitos protocolos de segurança essenciais, como **HTTPS** e **SSH**.

### 1.2 Princípio Básico de Operação

O fluxo de comunicação é baseado na posse das chaves:

1.  O destinatário (Alice) possui um par de chaves (Pública e Privada).
2.  O remetente (Bob) usa a **chave pública de Alice** para criptografar a mensagem.
3.  Apenas Alice pode descriptografar a mensagem usando sua **chave privada secreta**.

## 2. A Matemática por Trás do Algoritmo

O RSA se apoia firmemente na Teoria dos Números.

### 2.1 Aritmética Modular

A aritmética modular é fundamental para o RSA.
A expressão $a \equiv b \pmod n$ significa que $a \mod n = b \mod n$. A exponenciação modular, $(a^k) \mod n$, é a operação central e deve ser calculada de forma eficiente.

### 2.2 Números Primos e Fatoração

Os primos $p$ e $q$ são escolhidos na fase de geração de chaves.

*   O módulo $n$ é o produto desses primos ($n = p \times q$).
*   Encontrar $p$ e $q$, dado apenas $n$, é o que chamamos de **Problema da Fatoração**, e é esse desafio computacional que garante a segurança do RSA para números grandes.

### 2.3 Função Totiente de Euler ($\varphi(n)$)

A função $\varphi(n)$ conta quantos números menores que $n$ são **coprimos** com $n$. Para RSA, onde $n$ é o produto de dois primos distintos ($p$ e $q$):
$$\varphi(n) = (p-1) \times (q-1)$$
**Este valor ($\varphi(n)$) deve ser mantido em segredo**.

### 2.4 Teorema de Euler e Inverso Modular

O **Corolário do Teorema de Euler**, $a^{(k\times\varphi(n) + 1)} \equiv a \pmod n$, prova que a descriptografia funciona.

O **expoente privado** ($d$) é o inverso modular do expoente público ($e$) módulo $\varphi(n)$:
$$e \times d \equiv 1 \pmod{\varphi(n)}$$
Este valor é calculado usando o **Algoritmo Euclidiano Estendido**.

## 3. O Algoritmo RSA Passo-a-Passo

O processo RSA é dividido em três etapas críticas:

### Fase 1: Geração de Chaves

1.  **Gerar Primos ($p$ e $q$):** Gere dois números primos **distintos** de tamanhos similares. Para uma chave de $n$ bits, $p$ e $q$ devem ter $\approx n/2$ bits cada (ex: 256 bits para uma chave de 512 bits).
2.  **Calcular o Módulo ($n$):** $n = p \times q$. Este é o módulo público.
3.  **Calcular $\varphi(n)$:** $\varphi(n) = (p-1) \times (q-1)$.
4.  **Escolher Expoente Público ($e$):** Escolha $e$ tal que $1 < e < \varphi(n)$ e $\text{gcd}(e, \varphi(n)) = 1$. O valor **$e = 65537$** ($2^{16} + 1$) é o mais comum, pois é primo e torna a exponenciação mais eficiente (poucos bits '1').
5.  **Calcular Expoente Privado ($d$):** Encontre $d$ tal que $e \times d \equiv 1 \pmod{\varphi(n)}$ usando o Algoritmo Euclidiano Estendido.

**Resultado:**
*   **Chave Pública:** $(n, e)$.
*   **Chave Privada:** $(n, d)$.

### Fase 2: Criptografia

Para criptografar a mensagem $m$ (onde $m < n$) usando a chave pública $(n, e)$:

$$c = m^e \mod n$$
Onde $c$ é o texto cifrado.

### Fase 3: Descriptografia

Para descriptografar o texto cifrado $c$ usando a chave privada $(n, d)$:

$$m = c^d \mod n$$
Onde $m$ é a mensagem original. A prova de que isso funciona decorre diretamente do Corolário de Euler, garantindo que $(m^e)^d \equiv m \pmod n$.

## 4. Detalhes de Implementação Algorítmica

A viabilidade prática do RSA depende de algoritmos eficientes para números grandes.

### 4.1 Teste de Primalidade Miller-Rabin

É usado para gerar $p$ e $q$. É um algoritmo probabilístico: se passar em $k$ rounds de teste, a probabilidade de erro (aceitar um número composto como primo) é menor que $(1/4)^k$.

### 4.2 Exponenciação Modular Rápida

Também conhecido como algoritmo **"Square-and-Multiply"**.

*   **Função:** Calcular $a^b \mod n$ de forma eficiente.
*   **Complexidade:** $O(\log \exp)$ (logaritmo do expoente).
*   É crucial, pois evita que os resultados intermediários se tornem gigantescos, viabilizando o RSA.

### 4.3 Algoritmo Euclidiano Estendido (AEE)

Essencial para calcular o inverso modular de $e$ e, assim, determinar o expoente privado $d$. O AEE encontra os coeficientes $x$ e $y$ que satisfazem a identidade de Bézout: $ax + by = \text{gcd}(a, b)$.

## 5. Análise de Segurança e Tamanho das Chaves

A segurança do RSA é totalmente dependente da dificuldade do Problema da Fatoração.

### 5.1 O Problema da Fatoração e GNFS

O melhor algoritmo clássico conhecido para fatorar $n$ é o **General Number Field Sieve (GNFS)**. Para quebrar uma chave de 2048 bits, o GNFS exigiria aproximadamente $2^{112}$ operações, o que é inviável na computação atual.

### 5.2 Tamanhos de Chave Recomendados

O tamanho da chave determina o nível de segurança do algoritmo:

| Ano | Tamanho Mínimo (RSA) | Equivalência Simétrica | Status |
| :---: | :---: | :---: | :---: |
| 2010 | 1024 bits | 80 bits | ❌ Quebrado |
| 2015 | 2048 bits | 112 bits | ✅ **Seguro Atual** |
| 2025 | 3072 bits | 128 bits | ✅ **Recomendado** |
| 2030 | 4096 bits | 140 bits | 🔮 Futuro |

Para uso em produção, **chaves de 2048 bits ou maiores** são obrigatórias.

### 5.3 Ataques e Contramedidas

O RSA é robusto, mas vulnerável se a implementação for falha.

| Tipo de Ataque | Descrição | Contramedida |
| :--- | :--- | :--- |
| **Ataque de Padding** (ex: Bleichenbacher) | Explora falhas no esquema de preenchimento (padding). | Usar **Padding OAEP** (Optimal Asymmetric Encryption Padding) para criptografia. |
| **Ataque de Canal Lateral** (Side-Channel) | Analisa tempo de execução ou consumo de energia para deduzir o expoente privado $d$. | Implementar operações criptográficas em **tempo constante** (`constant_time_exp`). |
| **Expoente Privado Pequeno** | Se $d$ for muito pequeno, torna-se vulnerável ao ataque de Wiener. | Garantir que $d$ tenha um número de bits adequado (ex: maior que $n$ / 4). |

## 6. Otimizações e Limitações

### 6.1 Otimização: Chinese Remainder Theorem (CRT)

A descriptografia é a operação mais lenta do RSA. O CRT (Teorema Chinês do Resto) é usado para acelerar a descriptografia em aproximadamente **4x**.

*   O CRT divide o cálculo de $m = c^d \mod n$ em dois cálculos mais rápidos, um módulo $p$ e outro módulo $q$, e então combina os resultados.
*   Para isso, a chave privada otimizada deve armazenar valores pré-computados, como $p$, $q$, $d_p = d \mod (p-1)$ e $d_q = d \mod (q-1)$.

### 6.2 Limitações Fundamentais

1.  **Performance:** O RSA é **significativamente mais lento** ($\sim 1000\text{x}$) que algoritmos simétricos (como AES).
    *   **Uso Híbrido:** Por ser lento, o RSA é tipicamente usado apenas para criptografar uma chave simétrica temporária; esta chave simétrica, por sua vez, criptografa os dados grandes.
2.  **Tamanho da Mensagem:** A mensagem $m$ deve ser menor que o módulo $n$ ($m < n$). Para 2048 bits, isso limita a mensagem a cerca de 255 bytes por bloco.
3.  **Complexidade de Implementação:** O RSA é fácil de implementar incorretamente, exigindo atenção a muitos detalhes críticos para a segurança (como o uso correto de `padding` e a proteção contra *side-channels*).

### 6.3 Vulnerabilidade Quântica

O **Algoritmo de Shor** (1994) é capaz de fatorar o módulo $n$ em tempo polinomial.

*   Embora exija um computador quântico de grande escala (estimado em ~4096 qubits lógicos), essa ameaça deve se tornar prática em 10 a 20 anos.
*   **Contramedida:** Migração futura para criptografia pós-quântica (PQC), como algoritmos baseados em *Lattice* (Kyber, Dilithium), que já estão sendo padronizados pelo NIST.

## 7. Conclusões e Uso em Produção

| ✅ **USAR RSA PARA** | ❌ **NÃO USAR RSA PARA** |
| :--- | :--- |
| Troca de chaves simétricas (criptografia híbrida) | Criptografia de dados grandes ou volumes altos |
| Assinatura digital (usando esquema PSS) | Novos sistemas críticos (preferir ECC ou Pós-Quânticos) |

### Requisitos de Implementação Segura (Produção)

Implementações educacionais (como as que usam chaves de 512 bits e ignoram o *padding*) **não são seguras**. Para produção, é fundamental:

1.  Usar bibliotecas criptográficas testadas (ex: OpenSSL, BoringSSL).
2.  Garantir o uso de **chaves $\ge 2048$ bits**.
3.  Aplicar **Padding OAEP** para criptografia e **Padding PSS** para assinatura digital.
4.  Garantir que os dados sensíveis (como os primos $p$ e $q$, e o expoente privado $d$) sejam zerados da memória após o uso (limpeza de memória, `impl Drop`).
5.  Implementar proteções robustas contra ataques de canal lateral (timing attacks).

> 🎓 "O RSA nos ensina que a matemática pode ser tanto elegante quanto prática, fornecendo segurança através da beleza dos números primos."
```
// SIMULAÇÃO DE ESTRUTURA DE CÓDIGO FUNCIONAL EM RUST

use num_bigint::{BigInt, Sign, RandBigInt};
use rand::rngs::OsRng; // Para geração de números aleatórios seguros

// 1. Funções Auxiliares (Miller-Rabin, Euclidiano Estendido, Exp. Modular)

// fn exponenciacao_modular(base: &BigInt, exp: &BigInt, modulo: &BigInt) -> BigInt { ... } [21]
// fn algoritmo_euclidiano_estendido(a: &BigInt, b: &BigInt) -> (BigInt, BigInt, BigInt) { ... } [22]
// fn inverso_modular(e: &BigInt, phi: &BigInt) -> BigInt { ... } [7, 8]
// fn eh_primo(n: &BigInt, k: u32) -> bool { ... } (Implementa Miller-Rabin) [20]

// 2. Estruturas de Chave
struct PublicKey {
    n: BigInt, // Módulo
    e: BigInt, // Expoente público (e.g., 65537)
}

struct PrivateKey {
    n: BigInt,
    d: BigInt, // Expoente privado
}

// 3. Função de Geração de Chaves
fn generate_keys(bit_size: u64) -> (PublicKey, PrivateKey) {
    // 1. Gerar p e q (primos de bit_size/2) [6]
    // 2. Calcular n = p * q [6]
    // 3. Calcular phi_n = (p - 1) * (q - 1) [8]
    // 4. Definir e = 65537 [8]
    // 5. Calcular d = inverso_modular(e, phi_n) [8]
    // Retornar (PublicKey { n, e }, PrivateKey { n, d })
}

// 4. Criptografia
fn encrypt(message: &BigInt, pub_key: &PublicKey) -> BigInt {
    // c = message^e mod n
    // Usa exponenciacao_modular [9, 21]
}

// 5. Descriptografia
fn decrypt(ciphertext: &BigInt, priv_key: &PrivateKey) -> BigInt {
    // m = ciphertext^d mod n
    // Usa exponenciacao_modular [9, 21]
}

// 6. Teste Principal (main)
fn main() {
    let bit_size = 512; // Chave educacional [12]
    let (pub_key, priv_key) = generate_keys(bit_size);

    // Mensagem a ser criptografada (em formato BigInt)
    let original_message = BigInt::from(123456789);

    let ciphertext = encrypt(&original_message, &pub_key);

    let decrypted_message = decrypt(&ciphertext, &priv_key);

    // Validação
    assert_eq!(original_message, decrypted_message);
    println!("Descriptografia bem-sucedida!");
}
