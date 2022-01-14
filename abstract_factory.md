# Abstract Factory

O Abstract Factory consiste em uma classe que fabrica (daí o nome Factory) classes que tem um mesmo comportamento, porém os executam de maneira diferente. 

Por exemplo: todo veículo tem propriedades que são inerentes a ele como acelerar, reduzir a velocidade e buzinar. Porém um carro acelera de uma maneira diferente que um barco, assim como um caminhão e um avião desaceleram de maneira diferente entre eles. 

Caso eu quisesse desenvolver um simulador de veículos (seja lá o que isso significa), vale a pena eu criar uma classe que descreve as características de um veículo (classe abstrata) e classes que descrevem o comportamento específico de cada veículo (classes concretas). 

Vamos supor esse simulador funcione da seguinte maneira

- Usuário seleciona um veículo
- Veículo acelera
- Veículo buzina
- Veículo para

Dessa forma o programa terá a seguinte estrutura:

- Uma classe *VehicleFactory* que é responsável por fabricar os objetos de acordo com um parâmetro (nesse caso o tipo do veículo) e retorná-los para quem solicitou.
- Uma classe *Vehicle* que é responsável por descrever as características compartilhadas dos veículos (acelerar, parar e buzinar).
- Várias classes que herdam da *Vehicle* e que implementam as características dos veículos. No exemplo abaixo criei apenas as classe *Boat* e *Car*.
- Uma função principal que pede ao usuário qual o tipo do veículo que deseja simular e com base nessa escolha solicite a fabricação do objeto correspondente. Com o objeto (veículo) criado chama os métodos que acelera, faz barulho e para o veículo.

```python
# Define the basic features of a vehicle
class Vehicle:
    def accelerate(self):
        pass

    def stop(self):

        pass
    
    def make_sound(self):

        pass

class Car(Vehicle):

    def __init__(self):
        pass

    def accelerate(self):
        # Implements how a car accelerates
        pass

    def stop(self):
        # Implements how a car stops
        pass

    def make_sound(self):
        # Implements how a car make sounds
        pass

class Boat(Vehicle):

    def accelerate(self):
        # Implements how a boat accelerates
        pass

    def stop(self):
        # Implements how a boat stops
        pass

    def make_sound(self):
        # Implements how a boat make sounds
        pass

class VehicleFactory:

    def make(type):

        if(type == 'car'):
            return Car()
        elif(type == 'boat'):
            return Boat()

        raise 'Vehicle type not found'

def main():

    factory = VehicleFactory()

    vehicle_type = input('Choose your vehicle type: ')

    vehicle = factory.make(vehicle_type)

    vehicle.accelerate()
    vehicle.make_sound()
    vehicle.stop()

		print('Simulation ended.')

if __name__ == "__main__":
    main()
```

A grande vantagem deste padrão de projeto é abstrair os detalhes de implementação de determinados objetos da classe principal (função *main* neste caso) e permitir com que novos tipos de objetos sejam adicionados e implementados sem que a classe principal sofra alterações. 

No exemplo acima, caso quiséssemos adicionar um novo tipo de veículo, apenas teríamos que criar uma nova classe que herda as propriedades da classe *Vehicle,* implementar os seus métodos e adicioná-la na classe abstrata. Nenhuma alteração seria necessária na função *main.* 

Na vida real, esse padrão de projeto pode ser muito útil para a implementação de integrações com outros sistemas que tenham a mesma natureza. Por exemplo, em um sistema que busque o preço de carros em vários sites e retorne os resultados seria interessante criar uma classe abstrata com os métodos:

- Entrar no site
- Pesquisar um carro
- Retornar os valores

e classes concretas que implementem esses métodos para cada site que o sistema permita acessar. Dessa forma, a função principal (responsável por chamar cada integração) não precisa se preocupar com os detalhes de implementação que podem mudar de site para site. 

**Em resumo:** 

**Abstract Factory = separação de responsabilidades →mais facilidade no desenvolvimento e manutenção do projeto → escalabilidade**