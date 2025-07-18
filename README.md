# Simulação de Radar com Processamento de Sinais em Python

Este projeto apresenta uma simulação completa de um sistema de radar utilizando Python e conceitos de processamento digital de sinais para estimar a distância e a velocidade de múltiplos alvos. A metodologia é baseada nos exercícios do capítulo 10 do livro "Computer-Based Exercises for Signal Processing Using MATLAB 5".

## Visão Geral

O objetivo deste trabalho é modelar um sistema de radar capaz de:

- **Estimar a Distância**: Transmitindo um sinal Chirp e utilizando um Filtro Casado para realizar a compressão de pulso, identificando o atraso temporal dos ecos.
- **Estimar a Velocidade**: Transmitindo um sinal em Rajada (Burst) e medindo o desvio de frequência Doppler causado pelo movimento dos alvos.
- **Simulação Integrada**: Combinar as duas técnicas, utilizando uma rajada de pulsos Chirp para detectar múltiplos alvos em um cenário com ruído, e então processar os dados para estimar com precisão suas distâncias e velocidades.

## ✨ Principais Funcionalidades

- **Estimativa de Distância Precisa**: Utiliza a técnica de compressão de pulso com sinais Chirp e Filtro Casado para obter alta resolução na medição de distância.
- **Estimativa de Velocidade**: Mede a velocidade dos alvos através da análise do efeito Doppler no sinal de retorno.
- **Detecção de Múltiplos Alvos**: Capaz de simular e detectar múltiplos alvos com diferentes distâncias e velocidades.
- **Simulação Realista**: Inclui a adição de ruído gaussiano ao sinal recebido para testar a robustez do sistema.
- **Processamento Otimizado**: Demonstra uma técnica de subamostragem para otimizar o processamento do sinal para detecção de velocidade.

## � Como Funciona

### 1. Estimativa de Distância

A distância de um alvo é determinada medindo o tempo de ida e volta (T_d) de um sinal de radar.
R = (1/2) * c * T_d


**Sinal Transmitido (Chirp)**: Utilizamos um sinal Chirp, que é uma onda cuja frequência varia com o tempo. A sua fase tem uma característica quadrática.
s(t) = e^(jπWt²/T), -T/2 ≤ t ≤ T/2


![Sinal Chirp](https://i.imgur.com/eBwZ85x.png)  
*Figuras 1 e 2 do artigo*

**Filtro Casado e Compressão de Pulso**: O sinal de retorno é processado por um Filtro Casado (h(t) = s*(-t)). A convolução entre o sinal de retorno e o filtro casado resulta na função de autocorrelação do Chirp, que possui um pico agudo e bem definido.
y(t) = h(t) * s(t-T_d) ≈ T * sinc(W(t-T_d))

A posição (t=T_d) deste pico revela o atraso do sinal e, consequentemente, a distância do alvo.

![Saída do Filtro Casado](https://i.imgur.com/pZz8gAn.png)  
*Figura 7 do artigo*

### 2. Estimativa de Velocidade

A velocidade é estimada a partir do desvio de frequência Doppler (f_d), que ocorre quando o sinal é refletido por um alvo em movimento.
v = (f_d * c) / (2f_c)


**Sinal Transmitido (Burst)**: Para medir a velocidade, o radar emite um trem de pulsos, conhecido como sinal em rajada (Burst).

![Sinal em Rajada](https://i.imgur.com/W2d2Lza.png)  
*Figura 15 do artigo*

**Análise Espectral**: O movimento do alvo multiplica o sinal de retorno por uma exponencial complexa (e^(jω_dn)), o que causa um deslocamento de ω_d no espectro de frequência do sinal. Ao analisar o espectro do sinal recebido (via DTFT), podemos medir esse deslocamento e calcular a velocidade.

![Deslocamento Doppler no Espectro](https://i.imgur.com/j1JtD2A.png)  
*Figura 17 do artigo*

## 🛰️ Parâmetros da Simulação Final

A simulação final integra as técnicas de distância e velocidade, utilizando uma rajada de pulsos Chirp. Os parâmetros foram baseados em sistemas de radar de aeroporto.

| Parâmetro                  | Valor | Unidade       |
|----------------------------|-------|---------------|
| Frequência da Portadora (f_c) | 7     | GHz           |
| Duração do Pulso (T)       | 7     | μs            |
| Largura de Banda (W)       | 7     | MHz           |
| Taxa de Amostragem (f_s)   | 8     | MHz           |
| Período Interpulso         | 60    | μs            |
| Número de Pulsos na Rajada (L) | 11    | Adimensional  |
| Alcance Mínimo (R_min)     | 3750  | m             |
| Alcance Máximo (R_max)     | 7500  | m             |

*Tabela I do artigo*

## 🎯 Cenário e Resultados

### Cenário Simulado

Foram simulados 3 alvos com as seguintes características:

| Alvo   | Distância (m) | Velocidade (m/s) |
|--------|---------------|------------------|
| Alvo 1 | 4000          | +50              |
| Alvo 2 | 5500          | -80              |
| Alvo 3 | 6800          | +120             |

*Tabela II do artigo*

### Detecção e Precisão

Após o processamento do sinal de retorno com ruído, os picos de energia revelaram a posição dos alvos, e a análise de fase pulso a pulso (DTFT) revelou suas velocidades.

![Perfil de Distância](https://i.imgur.com/CGBiTzM.png)  
*Figura 21 do artigo: Detecção dos 3 alvos no perfil de distância*

Os resultados detectados foram extremamente próximos dos valores simulados, validando o sucesso da simulação.

| Alvo   | Distância Simulada | Distância Detectada | Velocidade Simulada | Velocidade Detectada |
|--------|--------------------|---------------------|---------------------|----------------------|
| Alvo 1 | 4000.00 m          | 4012.50 m           | 50.00 m/s           | 50.76 m/s            |
| Alvo 2 | 5500.00 m          | 5512.50 m           | -80.00 m/s          | -79.86 m/s           |
| Alvo 3 | 6800.00 m          | 6825.00 m           | 120.00 m/s          | 121.01 m/s           |

*Tabela V (comparativo), Tabela III (distâncias detectadas) e Tabela IV (velocidades detectadas)*
