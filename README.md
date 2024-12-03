# SimulacaoFisicaBasica

### AUTOR: Gabriel Coutinho Chaves nUSP15111760 gabriel.coutinho.chaves@usp.br 

## Descrição Básica do Projeto
Este projeto é uma simulação interativa criada para explorar os conceitos de movimento em curvas e planos inclinados, sob a influência de forças como gravidade e atrito. O objetivo é oferecer uma representação visual que permita entender o comportamento de um corpo deslizando ao longo de uma superfície curva, que no projeto em questão foi definida por uma Curva de Bézier de controle ajustável.

O projeto consiste em simular um corpo de teste, representado figurativamente pelo personagem Sonic, sob a ação da força gravitacional e de uma curva de Bézier ajustável. O corpo de teste pode deslizar sobre a curva, estando também sujeito à força de atrito. Durante a execução do programa, é possível ajustar os parâmetros das forças e interagir diretamente com o corpo de teste, alterando sua posição ao arrastá-lo com o mouse. Além disso, o programa conta com um botão de reset que reposiciona o corpo de teste e redefine o sistema para o estado inicial.

## A Simulação
Com esta simulação, é possível observar em tempo real como um objeto desliza ao longo de uma curva, em um ambiente bidimensional. A trajetória do corpo é afetada pelos seguintes fatores ajustáveis:

- A Posição do Corpo de Teste: A posição do corpo de teste pode ser alterada arrastando com o mouse 
- Gravidade: A força que impulsiona o movimento para baixo.
- Atrito: Que desacelera o movimento da bola ao longo da superfície.
- Forma da superfície: Controlada por pontos ajustáveis em uma curva de Bézier.

## Implementação

O projeto foi implementado em **Python**, utilizando os seguintes pacotes principais:

- **Pygame**: Para a renderização da interface gráfica e animação interativa.
- **NumPy**: Para cálculos matemáticos e operações vetoriais.

Esses pacotes foram escolhidos por sua eficiência e facilidade de integração, permitindo que a simulação seja fluida, visualmente atraente e computacionalmente eficiente.

O programa foi modularizado em diferentes arquivos ou scripts Python para melhorar a organização e facilitar a manutenção. Cada módulo foi projetado para conter funcionalidades específicas relacionadas com elementos distintos da execução principal, promovendo a reutilização de código e separação de responsabilidades. Os módulos utilizados foram:
- **config.py**: Este módulo define constantes essenciais como dimensões da tela e cores, além da função print_text, que facilita a exibição de textos renderizados em superfícies do pygame. A função é útil para debug e interface, permitindo personalização de fonte, cor, posição e alinhamento.
- **slider.py**: Este módulo implementa uma classe Slider, que representa um controle deslizante horizontal com funcionalidade de interação no pygame. O slider exibe uma barra ajustável dentro de um contêiner, permitindo calcular uma porcentagem com base na posição do controle. Ele inclui métodos para manipular eventos (handle_event), atualizar a posição (update), desenhar o slider e texto associado (draw), e retornar a porcentagem atual (get_percentage).
- **bezier_curve.py**: Este módulo implementa a classe BezierCurve, que representa e manipula curvas de Bézier no contexto do pygame. A classe inclui métodos para calcular pontos e vetores tangentes na curva (get_point, get_tangent), determinar o ponto mais próximo e o ângulo da tangente em relação ao eixo x (closest_point_and_tangent, closest_point_angle), e desenhar a curva e seus pontos de controle (draw, draw_control_points). Também permite interação com o mouse, como arrastar os pontos de controle para modificar a curva dinamicamente (handle_event).
- **ball.py**: A classe Ball representa um corpo de teste que interage com o ambiente simulado. Ela gerencia a posição, velocidade e estado da bola em resposta às forças aplicadas, como gravidade e atrito, além de detectar e reagir a colisões com a curva de Bézier. A classe também permite interação do usuário, como arrastar a bola com o mouse e resetar sua posição. Visualmente, exibe animações diferentes dependendo do estado da bola: rolando, parada ou levantada.

## Conceitos de Física e Modelo Matemático
O projeto utiliza conceitos fundamentais de física e matemática para modelar o movimento de um corpo (simplificado por um circulo) representado pela classe Ball em interação com uma curva de Bézier ajustável. Os principais conceitos e abordagens matemáticas envolvidos são:

### Movimento e Dinâmica
A esfera é submetida a forças gravitacionais e de atrito que afetam sua velocidade e posição ao longo do tempo. Essas forças são aplicadas através de cálculos baseados nas leis de Newton:

- Gravidade: Representada como uma força constante que acelera a esfera verticalmente para baixo. A aceleração gravitacional é ajustável no programa.
- Atrito: Simula a resistência do movimento ao longo da curva. Uma vez que a Foça de Atrito é proporcional à velocidade, o atrito aqui é simulado reduzindo proporcionalmente a velocidade tangencial da esfera.

Para descrever o deslizamento do corpo, o passo essencial foi encontrar o vetor tangente à Curva de Bezier no ponto de contato com o objeto. Primeiramente, para definir o ponto de contato do corpo com a curva, utilizou-se a norma euclidiana para medir a distância, seguida da minimização dessa distância para uma discretização da curva. Tendo em mãos o ponto mais próximo, o cálculo da tangente foi feito a partir uma aproximação da derivada da curva naquele ponto, utilizando o quociente diferencial do eixo x e y. 

A posição do objeto é ajustada para evitar interpenetração, usando o vetor normal ao ponto de contato, obtido a partir da tangente da curva de bezier, pela fórmula: 

A velocidade do objeto é projetada no vetor tangente da curva, simulando deslizamento suave sobre a superfície.

A curva é parametrizada utilizando o algoritmo de De Casteljau para calcular pontos intermediários e vetores tangentes. Isso permite modelar superfícies suaves e responder dinamicamente às mudanças nos pontos de controle ajustados pelo usuário.

O movimento da esfera é integrado numericamente utilizando incrementos de tempo (dt), que é tomado como o inverso do FPS (Frames Per Segundo).

Atualização da posição com base na velocidade.
Cálculo da interação com a curva em intervalos regulares.
Uso de interpolação para determinar os vetores normais e tangentes, fundamentais para simular colisões e deslizamento.
Ajustes pelo Usuário
Os usuários podem interagir com o sistema para ajustar os parâmetros de gravidade e atrito em tempo real, utilizando sliders. Isso afeta diretamente os cálculos físicos, permitindo observar mudanças de comportamento no movimento da esfera.


## Como Usar

### Instalação e Dependências

**Clone o repositório**  
-Certifique-se de ter o Git instalado no seu sistema. Em seguida, execute os comandos abaixo para clonar o repositório do projeto e navegar até o diretório clonado:

   ```bash
   git clone https://github.com/gabrielcoout/SimulacaoFisicaBasica.git
   cd SimulacaoFisicaBasica
```

Certifique-se de que o Python está instalado e verifique se você possui o Python na versão 3.6 ou superior. Caso contrário, faça o download e instale-o a partir do site oficial do Python.

- Instale as dependências
O repositório contém um arquivo requirements.txt, que lista os pacotes necessários para executar o projeto. Para instalar as dependências, execute o seguinte comando:

```bash
Copiar código
pip install -r requirements.txt
```

Para rodar a simulação básica, navegue até o diretório clonado e utilize o seguinte comando no terminal:
```bash
  python main.py
```
