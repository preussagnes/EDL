# Origem

O.NET Framework é uma plataforma única para desenvolvimento e execução de sistemas e aplicações criada pela Microsoft. Nela qualquer código gerado pode ser executado em qualquer dispositivo que possua esse framework, evitando assim problemas de compatibilidade. 
Isso é possível pela sua FCL(Framework Class Library, Conjunto de Bibliotecas Unificadas) que fazem os programas rodarem num CLR ( Common Language Runtime em portugûes, Ambiente de Execução Independente de Linguagem) em vez de um hardware.
Originalmente suas bibliotecas seriam escritas na linguagem Simple Managed C (SMC), com um compilador próprio, no entanto, em Janeiro de 1999, a Microsoft escolheu Anders Hejlsberg  para formar um time para a criação de uma nova linguagem chamada Cool (C-like Object Oriented Language), renomeada em 2000 para C#.
Sua primeira versão era muito parecida com Java, porém com poucos recursos. Foi evoluindo e em 2007, na sua terceira versão, saiu da sombra do Java e se estabeleceu como uma linguagem.

# Classificação
    
C# era uma linguagem inspirada principalmente em Java e C++, contudo teve outras influências como C, Eiffel, Object Pascal. Ela visa ser uma linguagem simples, moderna, fortemente tipada, de uso geral e orientada a objeto. É predominantemente estática, mas pode ser dinâmica usando a funcionalidade dynamic. A linguagem é usada para desenvolvimento web, mobile e aplicações desktop.


# Comparação 

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

#### C# 

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

Neste exemplo iremos comparar hierarquia usando um exemplo de um jogo simples. temos 3 inimigos e 3 aliados, incluindo o jogador. O Jogador irá atacar todos os personagens e utilizamos hierarquia para saber qual tipo de personagem que foi atacado. Em C# usamos o comando is, que irá dizer se uma instância é filha ou o próprio tipo. No entanto, em C++ foi necessário uma lista para armazenar todos os tipos e os ancestrais e enum para enumerar os tipos de classes. Além disso, uma função boleana que irá ser acessada toda vez que for feita uma pergunta para determinar o tipo e a descendência de uma instância.

#### C++

```c++
#include <iostream>
#include <list>
using namespace std;

///Enumerando todos os tipos existentes no meu jogo
enum tipo {t_tudo, t_aliado, t_inimigo, t_player, t_npc, t_zumbi, t_caveira, t_golen};

///class que representa tudo que existe nesse hipotetico mundo.
class Tudo
{
	public:

	list<int> linhagem;

	Tudo()
    {
        linhagem.push_back(t_tudo);
    }
};

//DeClarando o protopipo.
bool instance_is(Tudo* BB, int t);

///Classes Aliadas
class Aliado : public Tudo
{
	public:

	int HP; //vida do personagem.
	int STR; //força do personagem.
	
	Aliado()
    {
        linhagem.push_back(t_aliado);
    }
};

class Player : public Aliado
{
	public:

	Player()
	{
		linhagem.push_back(t_player);
		HP = 30;
		STR = 15;

	}
	void Atack(Tudo* p);
};

class NPC : public Aliado
{
	public:

	int TIPO; /// 1 = apenas dialogo 2 = capaz de atacar
	
	NPC(int T)
	{
		linhagem.push_back(t_npc);
		
		TIPO = T;

		if (T == 1)
		{
			HP = 0;
			STR = 0;
		}
		else
		{
			HP = 30;
			STR = 15;
		}

	}
};

///Classes Inimigas

class Inimigo : public Tudo
{
	public:

	Inimigo()
    {
        linhagem.push_back(t_inimigo);
    }
	int HP;
};

class Zumbi : public Inimigo
{
    public:
    
	Zumbi()
	{
		linhagem.push_back(t_zumbi);

		HP = 50;
	}
};

class Caveira : public Inimigo
{
    public:

	Caveira()
	{
		linhagem.push_back(t_caveira);

		HP = 45;
	}
};

class Golen : public Inimigo
{
	public:

	///Novo campo.
	int DEF;

	Golen()
	{
		linhagem.push_back(t_golen);

		HP = 45;
		DEF = 5;
	}
};

void Player::Atack(Tudo* p)
{
    ///Verifica primeiro se é um inimigo
    if (instance_is(p, t_inimigo))
    {
        string nomemostro;///Mostrarar o nome do msotro na tela.

        ///Mudando a referencia.
        Inimigo* c = (Inimigo*)p;

        ///Caso não for um golen, não é preciso verificar a DEF
        if (!instance_is(c ,t_golen))
        {
            ///Fazendo danos.
            c->HP -= STR;

            if (instance_is(c, t_zumbi))
                nomemostro = "Zumbi";
            else if (instance_is(c, t_caveira))
                nomemostro = "Caveira";
        }
        ///Caso seja um golen é necessário usar seu campo DEF
        else
        {
            ///Mudando a referencia.
            Golen* c = (Golen*)p;


            ///Fazendo dano.
            c->HP -= STR -(c->DEF);

            nomemostro = "Golen";
        }

        ///Escrevendo informação na tela.
        cout << "Player atacou " << nomemostro << " que agora esta com " << c->HP<< " de HP" << endl;
    }
    else if(instance_is(p ,t_aliado))
    {
        ///Fazendo os teste
        if (instance_is(p ,t_npc))
        {
            NPC* c = (NPC*)p;

            ///Analizando que tipo de NPC é.
            if (c->TIPO == 1)///Apenas aquelas q falam.
                cout << "Que ABSURDO!!! vc esta atacando alguem desarmado" << endl;
            else
            {
                cout << "Voce atacou um aliado, ele ira revidar" << endl;

                HP -= c->STR;

                cout << "Seu HP agora e " << HP << endl;
            }
        }
        else if (instance_is(p ,t_player))
        {
            cout << "Oxi! voce deve ser um estudando da UERJ." << endl;
        }
    }
}

///Verifica o tipo da insntancia
bool instance_is(Tudo* bb, int t)
{
	list<int>::iterator it;
	for(it = bb -> linhagem.begin(); it != bb -> linhagem.end() ; it++)
		if (*it == t)
			return(true);
	return(false);
}

int main()
{
	///Criando os aliados*/
	Player* joaquin = new Player();
	NPC* Mago = new NPC(2);
    NPC* Arqueiro = new NPC(2);
    NPC* Refen = new NPC(1);

    ///Criando os inimigos.
    Caveira* ENY1 = new Caveira();
    Zumbi* ENY2 = new Zumbi();
    Golen* ENY3 = new Golen();

    joaquin->Atack(ENY1);
    joaquin->Atack(ENY2);
    joaquin->Atack(ENY3);
    joaquin->Atack(ENY2);
    joaquin->Atack(ENY3);
    joaquin->Atack(Refen);
    joaquin->Atack(Mago);
    joaquin->Atack(joaquin);
}
```

#### C#

```c#
using System;
public class Jogo {
	public static void Main(){
		///Criando os aliados*/
		Player joaquin = new Player();
		NPC Mago = new NPC(2);
    	NPC Refen = new NPC(1);
    	
		///Criando os inimigos.
    	Caveira ENY1 = new Caveira();
    	Zumbi ENY2 = new Zumbi();
    	Golen ENY3 = new Golen();
    	
		//Brigas.
		joaquin.Atack(ENY1);
    	joaquin.Atack(ENY2);
    	joaquin.Atack(ENY3);
    	joaquin.Atack(ENY2);
    	joaquin.Atack(ENY3);
    	joaquin.Atack(Refen);
    	joaquin.Atack(Mago);
    	joaquin.Atack(joaquin);}
}

//Pai de todas as classes
public class Tudo {}

//Pai de todos inimigos
public class Inimigo : Tudo
{
	public int HP;//vida do personagem.
}

//Class Zumbi
public class Zumbi : Inimigo
{
	public Zumbi()
	{
		HP = 50;
	}
}
	
//Class Caveira
public class Caveira : Inimigo
{
	public Caveira()
	{
		HP = 45;
	}
}

//Class Golen
public class Golen : Inimigo
{
	//Novo campo
	public int DEF;
	
	public Golen()
	{
		HP = 45;
		DEF = 5;
	}
}

//Pai de todos  os aliados
public class Aliado : Tudo
{
	public int HP; //vida do personagem.
	public int STR; //ataque do personagem.
}

//Class NPC
public class NPC : Aliado
{
	//Tipo de NPC 1 = Apenas para dialogo, 2 = Tb combate
	public int TIPO;

	public NPC(int npc_tipe)
	{
		//Arrumando o tipo.
		TIPO = npc_tipe;
		
		if (TIPO == 1)
		{
			HP = 0;
			STR = 0;
		}
		else
		{
			HP = 30;
			STR = 15;
		}
	}
}

	
//Class player.
public class Player : Aliado
{
	public Player()
	{
		HP = 30;
		STR = 15;
	}
	public void Atack(Tudo p)
	{
		///Verifica primeiro se é um inimigo
    	if (p is Inimigo) 
        {
            ///Mudando a referencia.
			Inimigo c = (Inimigo)p;

			///Caso não for um golen, não é preciso verificar a DEF
			if (!(p is Golen))
			{
				///Fazendo danos.
				c.HP -= STR;
			}
			///Caso seja um golen é necessário usar seu campo DEF
			else
			{
				///Mudando a referencia.
				Golen g = (Golen)p;
				
				///Fazendo dano.
				g.HP -= STR - (g.DEF);
			}

			///Escrevendo informação na tela.
			Console.WriteLine("Player atacou " + p.GetType().Name + " que agora esta com " +c.HP +" de HP");
	    }
        else if(p is Aliado) //Não esquecer to Tudo
        {
            ///Fazendo os teste
            if (p is NPC)
            {
                NPC c = (NPC)p;

                ///Analizando que tipo de NPC é.
                if (c.TIPO == 1)///Apenas aquelas q falam.
                    Console.WriteLine("Que ABSURDO!!! vc esta atacando alguem desarmado");
                else
                {
                    Console.WriteLine("Voce atacou um aliado, ele ira revidar");
                    HP -= c.STR;
				    Console.WriteLine("Seu HP agora e " + HP);
                }
            }
            else if ( p is Player)     //Não esquecer to Tudo
            {
                Console.WriteLine("Oxi! voce deve ser um estudando da UERJ");
		    }
	    }
    }
}
```

### Assincronia

É uma ferramenta em C# que possibilita uma melhora no desempenho e aprimorar resposta de um aplicativo. Ele funciona de forma que quando um processo é bloqueado o aplicativo todo não para, somente a parte que depende desse processo.Por exemplo, quando um aplicativo acessa um recurso da Web que pode ser lento, se ele for bloqueado dentro de um processo síncrono todo o aplicativo deve esperar. Porém, se for assíncrono, o aplicativo pode prosseguir trabalhando em outra atividade que não depende da desse recurso da Web até o bloqueio terminar.

### Exemplo 3

AccessTheWebAsync, um método assíncrono, cria uma instância de HttpClient e chama o método assíncrono GetStringAsync para baixar o conteúdo de um site como uma cadeia de caracteres.
Caso ocorra um evento inesperado GetStringAsync é suspenso e para evitar o bloqueio de recursos, transfere o controle para seu chamador, AccessTheWebAsyn que continuará com outros trabalhos não dependente dele (DoIndependentWork).
Quando GetStringAsync completar e produzir um resultado de cadeia de caracteres. O resultado é armazenado na tarefa que representa a conclusão do método, getStringTask. O operador await recupera o resultado de getStringTask e retornado a urlContents.
Quando AccessTheWebAsync tem o resultado da cadeia de caracteres, o método pode finalmente calcular o comprimento da cadeia de caracteres e o manipulador de eventos de espera poderá retomar.

#### C#

![Code](https://i-msdn.sec.s-msft.com/dynimg/IC612215.png)
Format: ![Alt Text](url)

Fonte:
---
https://www.programacaoprogressiva.net/2012/08/comece-programar-linguagem-de_31.html
https://docs.microsoft.com/pt-br/dotnet/csharp/whats-new/csharp-version-history
https://pt.wikipedia.org/wiki/C_Sharp#Implementa%C3%A7%C3%B5es
https://en.wikipedia.org/wiki/C_Sharp_(programming_language)
https://pt.stackoverflow.com/questions/190463/o-que-%C3%A9-estilo-de-tipagem
https://pt.stackoverflow.com/questions/125588/c-%C3%A9-uma-linguagem-compilada-ou-interpretada
https://pt.wikipedia.org/wiki/Microsoft_.NET
http://www.guj.com.br/t/qual-a-diferenca-entre-c-c-c/331885
https://www.cprogramming.com/tutorial/constructor_destructor_ordering.html
https://www.oficinadanet.com.br/artigo/526/c_sharp_csharp_o_que_e_esta_linguagem
https://www.upwork.com/hiring/development/c-sharp-vs-c-plus-plus/
https://msdn.microsoft.com/pt-br/library/hh191443(v=vs.120).aspx
https://pt.stackoverflow.com/questions/21508/qual-a-diferen%C3%A7a-entre-uma-linguagem-de-programa%C3%A7%C3%A3o-est%C3%A1tica-e-din%C3%A2mica
