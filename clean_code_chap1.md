# Clean Code - Chapter 1

- Nunca vamos nos livrar de códigos e linguagens de programação
    - Código é a implementação dos detalhes de uma abstração/conceito/requisito
    - O nível de detalhe necessário em um requisito nunca pode ser muito abstrato
        - O que acarreta no fato de que temos que dizer o que a máquina deve fazer, que naturalmente implica em código
        - Detalhamento ⇒ código
    - Obviamente, as linguagens sempre evoluirão e consequentemente se tornarão mais abstratas.
        - Isso não significa que código deixará de existir
    - Tem um vídeo do Akita bem legal sobre o assunto
        - [https://www.youtube.com/watch?v=1RARFXh_aa0](https://www.youtube.com/watch?v=1RARFXh_aa0)
- Provavelmente uma equipe que está codando muito rápido vai perder qualidade ao longo do tempo
    - Pressa ⇒ código ruim
    - O autor relata no livro a história de uma empresa que lançou um ótimo produto, que várias pessoas compraram
        - Ao longo do tempo, a frequência das atualizações diminuiu e o número de bugs aumentava a cada release novo
        - O motivo: o calendário estava apertado demais e os programadores foram obrigados a fazerem código sem pensar
        - Resultado: manutenção e gerenciamento do código se tornaram extremamente difíceis e a empresa faliu
    - Esse é o famoso “arrumo outro dia, o código funciona e eu tenho mais 3895 tarefas pra entregar” ou “bora fazer funcionar primeiro, depois fazemos uma sprint de refatoração”.
- É o trabalho do programador defender a boa qualidade do código
    - Fazer um código ruim porque está sem tempo é como um médico receber a ordem de um paciente para não lavar as mãos antes da cirurgia. Claramente o paciente é o chefe, mas o médico não pode aceitar tal pedido.
    - É óbvio que devemos ter empatia com os stakeholders. Acredito que devemos encontrar o equilíbrio entre velocidade e qualidade.
        - As vezes pensar demais é ruim também. É fácil cair na famosa o*verengineering* quando fica uma semana pensando no problema.
- Quando o código está ruim a primeira ideia que vem na nossa cabeça é fazer começar do zero
    - Não é uma ideia uma vez que você vai ter que dividir o seu time em dois: um time para manter o código “legado” até o código novo estar pronto e outro para fazer o código novo
    - Problemas do time do código novo:
        - devem fazer um sistema que faz tudo que o sistema antigo faz
        - devem adicionar todas as novidades / alterações que foram feitas no código antigo enquanto o novo não estava pronto
- Um código ruim é como um prédio com janelas quebradas: parece que as pessoas não se importam ele. Assim, outras pessoas não se importarão e mais janelas serão quebradas.
- Bom código = código focado = faz uma coisa bem
    - As classes, módulos e funções devem ter um escopo muito bem definido.
    - Essas partes não podem conter detalhes do código ao entorno dela
    - Conceito criado por Bjarne Stroustrup (criador do C++)
- Bom código = código que outros programadores podem entendê-lo e aprimorá-lo
    - Código dever ser lido como uma história
- Bom código = código com expressividade
    - Ou seja, o código diz o que ele está fazendo
    - Essa afirmação é uma junção das duas anteriores
        - Código focado ^ código bem descrito ⇒ código expressivo
- Bom código = código sem duplicações
    - Duplicações significam que a arquitetura está conceitualmente defasada
        - Se tem código duplicado, é provável que exista uma classe/função pedindo para ser criada
- Lemos código muito mais do que escrevemos código.
- Lei dos escoteiros: mantenha o código mais limpo do que você o encontrou
    - Se entrar em um módulo e ver que melhorias podem ser feitas, faça
