# Lei de Wirth

- O desenvolvimento de software não aumenta de velocidade igual o hardware
    - Temos um problema intrínseco no desenvolvimento

“There is no single development, in either technology or management technique, which by itself promises even one order of magnitude [tenfold] improvement within a decade in productivity, in reliability, in simplicity. We cannot expect ever to see two-fold gains every two years in software development, as there is in hardware development” Fred Brooks - Mythical Man Month

A tese de Brooks é que existem dos tipos de problemas no desenvolvimento de software:

- Acidentais: problemas que o programador cria e que são fáceis de resolver. Um exemplo seria acessar um endereço de memória errado ou criar um loop infinito dentro do código. Corrigi-los é fácil: basta ir no código, encontrar o erro e arrumá-lo. Além disso, existe uma infinidade de tecnologias que permitem com que esse tipo de erro seja facilmente mitigado, como linguagens de programação mais abstratas, IDEs, debbugers e compiladores.
- Essenciais: são os problemas que estão diretamente ligados ao problema que queremos resolver e, principalmente, na maneira que vamos resolver. Diferentemente dos problemas acidentais, que estão na camada da implementação da solução (código), os problemas essenciais estão na camada de arquitetura da solução (regras de negócio, componentes, módulos e interação entre as partes do sistema). É extremamente comum projetos atrasarem porque regras de negócios não foram levantadas previamente ou porque um módulo novo quebra todo o funcionamento anterior do sistema.

Brooks afirma que esta dificuldade vem do fato do software ser abstrato para o cérebro humano. Na engenharia civil, onde os componentes que formam o produto final são visíveis e tangíveis (você literalmente consegue pegar, tocar e visualizar) é mais fácil identificar falhas estruturais que no futuro podem causar problemas. Na engenharia de software, temos que lidar com conceitos e estruturas que a cognição humana tem dificuldades de enxergar. 

Por ser uma área do conhecimento nova (menos de cem anos), a engenharia de software ainda não amadureceu o suficiente para ter um conjunto de métodos e disciplinas bem definidas que permitam que o problema essencial seja abordado com agilidade. Para efeito de comparação: a engenharia civil teve suas primeiras grandes obras em 2500 A.C, ou seja, estamos quase 4500 anos atrasados em relação a conhecimento acumulado pela experiência. 

Essa tese de Fred Brooks mostra o tremendo desafio que temos ao desenvolver e arquitetar software e é um dos motivos que me faz gostar tanto da área. 

-* No livro o autor cita técnicas e métodos que buscar reduzir os efeitos da dificuldade essencial, mas isso vou deixar para outro postagem