# Origem

O.NET Framework é uma plataforma única para desenvolvimento e execução de sistemas e aplicações criada pela Microsoft. Nela qualquer código gerado pode ser executado em qualquer dispositivo que possua esse framework, evitando assim problemas de compatibilidade. 
Isso é possível pela sua FCL(Framework Class Library, Conjunto de Bibliotecas Unificadas) que fazem os programas rodarem num CLR ( Common Language Runtime em portugûes, Ambiente de Execução Independente de Linguagem) em vez de um hardware.
Originalmente suas bibliotecas seriam escritas na linguagem Simple Managed C (SMC), com um compilador próprio, no entanto, em Janeiro de 1999, a Microsoft escolheu Anders Hejlsberg  para formar um time para a criação de uma nova linguagem chamada Cool (C-like Object Oriented Language), renomeada em 2000 para C#.
Sua primeira versão era muito parecida com Java, porém com poucos recursos. Foi evoluindo e em 2007, na sua terceira versão, saiu da sombra do Java e se estabeleceu como uma linguagem.

# Classificação
    
C# era uma linguagem inspirada principalmente em Java e C++, contudo teve outras influências como C, Eiffel, Object Pascal. Ela visa ser uma linguagem simples, moderna, fortemente tipada, de uso geral e orientada a objeto. É predominantemente estática, mas pode ser dinâmica usando a funcionalidade dynamic. A linguagem é usada para desenvolvimento web, mobile e aplicações desktop.


# Comparação 

C, C++ vs C#

Algumas diferenças são que em C# ponteiros e aritmética precisam ser checadas ou seram usadas no modo inseguro (unsafe mode). Normalmente os acessos a objetos são realizados através de referências e são liberados através da coleta de lixo (garbage collector). A sintaxe para a declaração de vetores é diferente (exemplo 2).
        C# não possui destrutores usados para deletar objetos e nem herança múltipla como em C++. O mais próximo seria a interface disposable unida com a construção using block. Também não é permitida herança múltipla, mas uma classe pode implementar várias interfaces abstratas.


### Exemplo 1

Podemos observar que a sintáxe de C#  se assemelha a Java.

#### Java

```java
public class HelloWorld 
{
    public static void main(String[] args) 
    {
        System.out.println("Hello, World");
    }
}
```


#### C 

```c
#include <stdio.h> 
int main() 
{ 
    printf ("Hello World"); 
    return 0; 
} 
```

C# 

```c#
using System; 
class HelloWorld 
{ 
    public static void Main(String args[]) 
    { 
    Console.WriteLine("HelloWorld"); 
    } 
}
```


### Exemplo 2

#### C++

```c++
#include <iostream>
using namespace std;


int main()
	{
	int N;
	
	cin >> N;
	int Array[N];
	
	
	//Impossivel fazer Array.Length, Logo a variavel N deve ser guardad a fim q se percorar o array.
	cout << "O array tem tamnho" << N <<"\n";
	
	//Ou se dividir o tamanho todo do array pelo tamanho do seu tipo.
	cout << "O array tem tamnho" << sizeof(Array)/sizeof(int) <<"\n";
	
	
	return(0);
	}
```

#### C#

```c#
using System;

public class Program
{
	public static void Main(String [] agrs)
	{
		Console.WriteLine("Escreva o tamanho do array:");
		int N = Convert.ToInt32(Console.ReadLine());
		int[] Array = new int[N];
		
		Console.WriteLine("O tamanho do arry eh:");
		Console.WriteLine(Array.Length);
		
	}
}
```

Nota-se duas maneiras diferentes de trabalhar com arrays. Em C# arrays já declarados são objetos/classes e são um espaço contínuo na memória. Logo, é possível saber o seu tamanho acessando o campo Length.
Em c++ um array é apenas um alocamento de memória contínuo, logo, quando é alocado por variáveis é difícil obter o tamanho dele. No entanto, pode-se ser obtido através do conhecimento do tamanho do array em tempo de execução, mas como descobrir o tamanho de um array genérico?
A primeira opção é guardar a variável (no exemplo, N) e a partir dessa informação será possível saber o tamanho do array, com tudo isso, pode conter falta de expressão do programador inexperiente, pois se por acaso ele usar alguma função que tenha como parâmetro esse array, ele necessitará passar essa variável como parâmetro também. Uma forma mais eficiente seria usar sizeof no identificador do array que retornará o seu comprimento multiplicado pelo tamanho do tipo do array, basta dividir pelo tamanho do tipo obtendo assim seu tamanho.

### Exemplo 3

#### C++

```c++
#include <iostream>
using namespace std;


class Pessoa
	{
	public:
	
	int idade;
	string nome;
	
	
	void Inicialisador(string n, int nu)
		{
		nome = n;
		idade = nu;	
		}
	};

	
  
void AlteraC(Pessoa A)
	{
	A.idade = 360;
	A.nome = "JOJO";
	}
	
void AlteraC(Pessoa* A)
	{
	A->idade = 360;
	A->nome = "JOJO";
	}


int main()
	{
	Pessoa P; 
	
	P.Inicialisador("Jhonatan", 100);  
	cout << P.idade<<" "<< P.nome<<"\n";
	
	AlteraC(P);
	cout << P.idade<<" "<< P.nome<<"\n";
	
	AlteraC(&P);
	cout << P.idade<<" "<< P.nome<<"\n";
	
	return(0);
	}
```

#### C#

```c#
using System;
					
public class Program
{
	public static void Main(String [] agrs)
	{
		Pessoa A = new Pessoa(100 ,"Jhonatan");
		Console.WriteLine(A.Idade + " " +A.Nome);
		
		MudaPessoa(A);
		Console.WriteLine(A.Idade + " " +A.Nome);
			
	}
	
	public static void MudaPessoa(Pessoa Pes)
		{
		Pes.Idade = 360;
		Pes.Nome = "JOJO";
	
		}
		
}


public class Pessoa
	{
	public int Idade;
	public string Nome;

	public Pessoa(int A, string N)
		{
		Idade = A;
		Nome = N;
		}
	}
```

Vemos em c++ a declaração de uma classe e a manipulação de um objeto criado, porém há um pequeno detalhe que programadores inexperientes podem deixar passar que é o parâmetro por referência ou valor. 
Note que na primeira função "AlteraC" (onde se passa um valor) caso o programador tenha a intenção de modificar o objeto,isso não ocorrerá, porque ao chamar a função com valor, se faz uma nova declaração da classe pessoa onde o argumento A não terá o mesmo endereço do valor P na main().  Qualquer mudança dentro desse escopo será feita apenas no valor copiado.
Para contornar essa situação basta passar uma referência à esse objeto, ou seja, não será uma cópia e sim o endereço,  sendo possível alterar qualquer dado referente a esse objeto
dentro do escopo da própria função.
Em c# não há necessidade de se preocupar em tratamento por referência ou valor, pois todo identificador de objeto/classe sempre é uma referência. Podemos transferir um ponteiro em vez do valor.
Os desenvolvedores normalmente optam por trabalhar com objetos usando referências. Esse método usado em C# é de grande ajuda. Todavia ocorrerão menos erros léxicos na hora de se programar, porém restringirá a maneira livre do programador se expressar.
    
Fonte:
---

https://www.programacaoprogressiva.net/2012/08/comece-programar-linguagem-de_31.html 

https://docs.microsoft.com/pt-br/dotnet/csharp/whats-new/csharp-version-history 

https://pt.wikipedia.org/wiki/C_Sharp#Implementa%C3%A7%C3%B5es

https://en.wikipedia.org/wiki/C_Sharp_(programming_language)

https://pt.stackoverflow.com/questions/190463/o-que-%C3%A9-estilo-de-tipagem

https://pt.stackoverflow.com/questions/125588/c-%C3%A9-uma-linguagem-compilada-ou-interpretada

https://pt.wikipedia.org/wiki/Microsoft_.NET

https://pt.stackoverflow.com/questions/125588/c-%C3%A9-uma-linguagem-compilada-ou-interpretada

http://www.guj.com.br/t/qual-a-diferenca-entre-c-c-c/331885

https://www.cprogramming.com/tutorial/constructor_destructor_ordering.html

https://www.oficinadanet.com.br/artigo/526/c_sharp_csharp_o_que_e_esta_linguagem

https://www.upwork.com/hiring/development/c-sharp-vs-c-plus-plus/
