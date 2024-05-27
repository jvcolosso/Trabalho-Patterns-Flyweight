# 🪰 Padrão de Design Flyweight

## O Que é o Padrão Flyweight?

O padrão Flyweight é um padrão de design estrutural que tem como objetivo otimizar o uso de memória ou recursos computacionais em situações onde um grande número de objetos similares precisa ser criado. Ele alcança isso compartilhando partes comuns de objetos entre várias instâncias, em vez de armazenar essas partes em cada objeto individualmente.

## Problema

Quando um sistema precisa lidar com muitos objetos semelhantes, cada um contendo partes que poderiam ser compartilhadas entre várias instâncias, pode ocorrer um consumo excessivo de memória. Por exemplo, em um editor de texto, cada caractere pode ser representado como um objeto. No entanto, muitos caracteres, como letras do alfabeto, podem ser idênticos em termos de fonte, tamanho e cor. Criar um objeto separado para cada caractere resultaria em um desperdício de memória.

## Solução

O padrão Flyweight sugere dividir os objetos em duas partes:

1. **Estado Intrínseco**: Este é o estado compartilhado entre várias instâncias de objetos. Ele é imutável e pode ser compartilhado sem alterações entre os objetos. Por exemplo, no caso do editor de texto, o estado intrínseco de um caractere pode incluir sua fonte, tamanho e cor.

2. **Estado Extrínseco**: Este é o estado específico de cada instância de objeto e pode variar de uma instância para outra. Ele é fornecido externamente quando necessário e não é armazenado dentro do objeto flyweight. No caso do editor de texto, o estado extrínseco de um caractere pode incluir sua posição na tela.

## Componentes do Padrão

- **Flyweight (Mosca)**: Define a interface para os objetos flyweight e contém o estado intrínseco compartilhado entre várias instâncias.
- **ConcreteFlyweight (Mosca Concreta)**: Implementa a interface Flyweight e armazena o estado intrínseco. Este objeto é compartilhado entre várias instâncias.
- **Factory (Fábrica)**: Cria e gerencia objetos flyweight. Garante que os objetos flyweight sejam compartilhados adequadamente.
- **Cliente**: Usa objetos flyweight e, quando necessário, fornece o estado extrínseco.

## Vantagens

- **Economia de memória**: Ao compartilhar partes comuns de objetos, o padrão Flyweight reduz significativamente o uso de memória.
- **Melhor desempenho**: Com menos objetos para gerenciar, há uma redução no tempo de criação e manipulação de objetos.
- **Flexibilidade**: Permite a personalização de objetos individuais, mantendo partes compartilhadas onde possível.

## Exemplo de Aplicação

Um exemplo comum de aplicação do padrão Flyweight é em editores gráficos, onde elementos gráficos como círculos, retângulos e linhas são frequentemente utilizados em grande quantidade. O padrão Flyweight pode ser usado para compartilhar propriedades comuns, como cor e estilo, entre esses elementos, economizando memória.

## Diagrama de Classes
![Diagrama de Classes](https://github.com/jvcolosso/Trabalho-Patterns-Flyweight/blob/main/diagrama-flyweight.png)

## Explicação do Código

- **Flyweight**: é a interface que define o método `draw()`. Serve como base para os objetos flyweight.
  
- **ConcreteFlyweigh**: é uma classe concreta que implementa a interface `Flyweight`. Ela possui o atributo `color`, representando o estado intrínseco. Esta classe é responsável por desenhar o objeto com base nos parâmetros fornecidos.
  
- **FlyweightFactory**: é responsável por criar e gerenciar objetos flyweight. Mantém um mapa de círculos existentes, permitindo a reutilização de objetos. Quando solicitado, fornece instâncias de objetos flyweight com base na cor especificada.
  
- **Client**: é a classe cliente que utiliza a fábrica para obter instâncias de flyweight e chamar o método `draw()`. Ela configura os parâmetros extrínsecos (posição e tamanho) e solicita a renderização do objeto.

Este diagrama representa a estrutura básica do padrão Flyweight, conforme discutido no seu trabalho. Você pode ajustar e expandir este código conforme necessário para representar com mais detalhes a implementação específica em JavaScript e os detalhes das classes.


## Implementação em JavaScript

```javascript
// Flyweight Interface: define operações que usam estado intrínseco e extrínseco
class Shape {
    draw() {}
}

// ConcreteFlyweight: implementação da interface Flyweight
class Circle extends Shape {
    // Estado intrínseco: compartilhado entre múltiplos objetos e imutável
    constructor(color) {
        super();
        this.color = color;
    }

    // Métodos para definir o estado extrínseco
    draw(x, y, radius) {
        // Usa estado intrínseco e extrínseco para realizar a operação
        console.log(`Circle: Draw() [Color: ${this.color}, x: ${x}, y: ${y}, radius: ${radius}]`);
    }
}

// FlyweightFactory: gerencia e compartilha objetos flyweight
class ShapeFactory {
    constructor() {
        // Mapa que armazena objetos flyweight existentes, permitindo reutilização
        this.circleMap = {};
    }

    // Método para obter um objeto flyweight
    getCircle(color) {
        // Verifica se já existe um círculo com a cor especificada
        let circle = this.circleMap[color];

        // Se não existir, cria um novo, armazena no mapa e retorna
        if (!circle) {
            circle = new Circle(color);
            this.circleMap[color] = circle;
            console.log(`Creating circle of color: ${color}`);
        }
        return circle;
    }
}

// Cliente: demonstra o uso do padrão Flyweight
class FlyweightPatternDemo {
    constructor() {
        // Conjunto de cores que serão usadas para criar círculos
        this.colors = ["Red", "Green", "Blue", "White", "Black"];
        this.shapeFactory = new ShapeFactory();
    }

    // Função auxiliar para obter uma cor aleatória do conjunto de cores
    getRandomColor() {
        return this.colors[Math.floor(Math.random() * this.colors.length)];
    }

    // Função auxiliar para gerar uma coordenada X aleatória
    getRandomX() {
        return Math.floor(Math.random() * 100);
    }

    // Função auxiliar para gerar uma coordenada Y aleatória
    getRandomY() {
        return Math.floor(Math.random() * 100);
    }

    run() {
        // Cria e desenha 20 círculos com estados extrínsecos diferentes
        for (let i = 0; i < 20; ++i) {
            // Obtém um círculo da fábrica, compartilhando instâncias conforme necessário
            const color = this.getRandomColor();
            const circle = this.shapeFactory.getCircle(color);

            // Define o estado extrínseco do círculo
            const x = this.getRandomX();
            const y = this.getRandomY();
            const radius = 100;

            // Desenha o círculo com o estado configurado
            circle.draw(x, y, radius);
        }
    }
}

// Execução
const demo = new FlyweightPatternDemo();
demo.run();



