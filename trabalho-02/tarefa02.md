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

O exemplo a seguir é de hierarquia. A primeira diferença notável é a forma de se declamar classe, em C++ foi necessário uma lista para armazenar todos os tipos e os ancestrais e enum para enumerar os tipos de classes. Além disso, uma função boleana que irá ser acessada toda vez que for feita uma pergunta para determinar o tipo e a descendência de uma instância. Paralelamente, o equivalente em C# somente necessita a função is ( similar a instance_of em java) na consulta já saberemos sua ascendência.


#### C++

```c++
// typeid, polymorphic class
#include <iostream>
#include <list>
using namespace std;
//Types.
enum types { tp_filho, tp_deriva2, tp_deriva1, tp_base};
//Definição das classes e suas herancas
class Base 
{	 	
	public:
		//Lista usada para guardar todos os acestrais da instancia.
		list<int> linhagem;
		Base()
		{	
			linhagem.push_back(tp_base);
		}
	};
class Deriva1 : public Base 
{
	public:
		Deriva1()
		{
			linhagem.push_back(tp_base); 
			linhagem.push_back(tp_deriva1);
		}
};
class Deriva2 : public Base 
{
	public:
		Deriva2()
		{
			linhagem.push_back(tp_base);
			linhagem.push_back(tp_deriva2);
		}
};
class Filho  : public Deriva1 
{
	public:
		Filho()
		{
			linhagem.push_back(tp_base);
			linhagem.push_back(tp_deriva1); 
			linhagem.push_back(tp_filho);
		}
};
//Função usada para testar se uma referencia é de um dado tipo	
bool instance_is(Base* bb, int t)
{
	list<int>::iterator it;
	for(it = bb->linhagem.begin(); it != bb->linhagem.end() ; it++)
		if (*it == t)
			return(true);
	return(false);		
}
int main () 
{
    //Inicia todos os objetos.
    Base* A = new Base;
    Deriva1* B = new Deriva1;
    Deriva2* C = new Deriva2;
    Filho* D = new Filho;
    //Testando so tipos.
    if (instance_is(A, tp_base))
        cout << "A tb é uma base\n";
    if (instance_is(A, tp_deriva1))
        cout << "A tb é um deriva1\n";
    if (instance_is(B, tp_base))
        cout << "B tb é uma base\n";
    if (instance_is(B, tp_deriva1))
        cout << "B tb é uma deriva1\n";
    if (instance_is(B, tp_deriva2))
        cout << "B tb é uma derivada2\n";
    if (!instance_is(C, tp_deriva1))
        cout << "C não é um derivada1\n";
    if (instance_is(D, tp_base) && instance_is(D, tp_filho) && instance_is(D, tp_deriva1))
        cout << "D é base, deriva1 e filho\n";
    return 0;

}
```

#### C#

```c#

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text.RegularExpressions;

namespace Rextester
{
    public class Program
    {
        public static void Main(string[] args)
        {
	    //Inicia todos os objetos.
            Base A = new Base();
            Herda1 B = new Herda1();
            Herda2 C = new Herda2();
            Filho D = new Filho();
	    //Perguntando qual é o tipo da classe ?
            if(A is Base)
                Console.WriteLine("A é uma base");
            if (A is Herda1)
                Console.WriteLine("A é um Herda1");
            if (B is Base)
                Console.WriteLine("B que é um" +B.GetType()  +"tb é uma base");
            if (B is Herda2)
		Console.WriteLine("B que é um" +B.GetType()  +"tb é uma Herda2");
            if (D is Herda1)
		Console.WriteLine("D que é um" +B.GetType()  +"tb é uma Herda1");
            if (D is Base)
		Console.WriteLine("D que é um" +B.GetType()  +"tb é uma Base");
        }
	//Criando as classes e atribuindo as heranças
        public class Base{};
        public class Herda1: Base{};
        public class Herda2: Base{};
        public class Filho: Herda1{}; 
    }
}
```



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
