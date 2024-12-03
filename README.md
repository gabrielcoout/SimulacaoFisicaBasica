# SimulacaoFisicaBasica

### Autor: Gabriel Coutinho Chaves  
**nUSP:** 15111760  
**Email:** gabriel.coutinho.chaves@usp.br  


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

### Gravidade  
No código, a gravidade é implementada adicionando uma aceleração vertical constante à velocidade do objeto em cada quadro da simulação. Especificamente, na função `update` da classe `Ball`, a linha `self.velocity[1] += GRAVITY * dt` aplica essa força ao vetor de velocidade vertical no objeto. O valor de gravidade (`GRAVITY`) é ajustável, permitindo observar os efeitos dessa força em diferentes cenários, como ambientes de baixa gravidade ou alta gravidade. Na execução principal, a aceleração é calculada com base na constante `BASE_GRAVITY`, que converte a gravidade em metros por segundo ao sistema de unidades da simulação. O valor de gravidade pode ser ajustado dinamicamente pelo usuário através de um controle deslizante (`slider1`). No loop principal, a gravidade é recalculada com base na porcentagem definida no slider (`gravity = slider1.get_percentage() * BASE_GRAVITY`).  

### Atrito  
O atrito, no contexto do código, é modelado como uma força proporcional à velocidade tangencial do objeto. Após projetar a velocidade no vetor tangente da curva, a função `update` aplica o atrito pela multiplicação do vetor velocidade por um fator redutor, `self.velocity *= (1 - FRICTION)`. Esse cálculo simula a resistência que desacelera o movimento ao longo da curva. O fator de atrito (`FRICTION`) também pode ser ajustado em tempo real pelo usuário. Ele pode ser ajustado dinamicamente por um controle deslizante (`slider2`). No loop principal, o atrito é recalculado com base na porcentagem definida pelo slider (`friction = slider2.get_percentage() / 200`). Essa força de resistência é aplicada ao vetor de velocidade após projetar o movimento da bola no vetor tangente da curva de Bézier.  

### Interação com a Curva  
A interação entre a bola e a curva de Bézier é central para a simulação e é gerenciada em várias etapas:  

1. **Detecção do Ponto de Contato:**  
   A posição mais próxima da bola na curva é determinada pela função `closest_point_and_tangent` da classe `BezierCurve`. Essa função amostra pontos na curva e calcula a menor distância até a posição da bola.  

2. **Ajuste de Posição:**  
   Quando a distância da bola até a curva é menor que o raio da bola, a posição da bola é ajustada para evitar interpenetração, utilizando o vetor normal ao ponto de contato.  

3. **Projeção da Velocidade:**  
   Após corrigir a posição, a velocidade da bola é projetada no vetor tangente ao ponto de contato na curva, simulando deslizamento suave. Em seguida, o efeito do atrito é aplicado à velocidade projetada.  

### Dinâmica do Sistema  
O sistema opera com base no controle do tempo (`dt`), definido como o inverso do FPS (quadros por segundo). Cada quadro é processado por meio de um loop principal que realiza as seguintes operações:  

- Processa eventos do usuário, como interações com sliders ou a curva de Bézier.  
- Calcula a gravidade e o atrito com base nos controles deslizantes.  
- Atualiza a posição e velocidade da bola com base nas forças aplicadas e na interação com a curva de Bézier.  
- Renderiza os elementos na tela, incluindo a bola, a curva de Bézier e os controles.  

### Controle da Curva de Bézier  
A curva de Bézier é definida por pontos de controle que podem ser ajustados pelo usuário. A interação ocorre quando o usuário clica e arrasta um ponto de controle com o mouse, e a nova posição é atualizada em tempo real. A curva é renderizada suavemente usando o algoritmo de De Casteljau, e a espessura e cor da linha são personalizáveis (`width=7` e `color=GRAY`).  

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
