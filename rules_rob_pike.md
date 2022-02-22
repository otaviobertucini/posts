# Rules by Rob Pike

- Otimização precoce: somente otimize o sistema quando for encontrado um gargalo. Não é recomendado palpitar onde sistema ficará lento na fase de implementação.
- Medição de velocidade: sempre busque os gargalos através de medições de desempenho do sistema. Nunca tome decisões sem dados que comprovem suas suspeitas.
- Algoritmos: sempre como preferência algoritmos simples, mesmo que seja mais lento que outros mais sofisticados. Como o número de elementos de uma estrutura de dados geralmente é pequeno, a diferença entre um algoritmo $O(n)$ e um $O(n^2)$ é insignificante.

É interessante notar que a segunda e a terceira regra são a continuação natural da primeira regra. Para encontrar gargalos, é necessário medição de performance. E dar preferência a algoritmos mais simples é o inverso de otimização precoce.