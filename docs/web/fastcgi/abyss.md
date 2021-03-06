---
title: Abyss
redirect_from:
  - /FastCGI_Abyss/
---

Informação de como configurar o [FastCGI] (/docs/web/fastcgi/) com suporte ao Abys Server.

Introdução
------------

[Abyss Web Server](http://www.aprelium.com/) é um servidor web rico em recursos e fácil de usar. No entanto de fonte fechado , O servidor X1 é *"um software grátis e funcional : sem telas promocionais, sem limitação de tempo, sem spyware, e sem propagandas."* ([Download](http://www.aprelium.com/abyssws/download.php))

 Ele realmente foca no "uso fácil", Abyss foi o servidor mais fácil de configurar. Ele foi preponderante em sua simplicidade e centro de controle integrado.

### Configurado e testado em:

-   [OpenSuSE 10.2](http://en.opensuse.org/OpenSUSE_News/10.2-Release) (Abyss X1)
-   *se você tiver testado alguma configuração adicional,por favor envie-me um email  [brian.nickel@gmail.com](mailto:brian.nickel@gmail.com).*

Alertas Gerais
----------------

Antes de fazer qualquer coisa, você deve ler a seção [FastCGI#Important_Information](/docs/web/fastcgi/#important-information) no site principal do FastCGI.

### Geral Passo 1: Configurar o Interpretador

Ao iniciar o servidor web Abyss, um centro de controle do servidor web será iniciado por padrão no endereço
<http://localhost:9999>. Simplesmente abra seu navegador neste endereço e siga os passos abaixo:

1.  Clique em "Configure" no host que você deseja adicionar suporte a ASP.NET
2.  Clique em "Scripting Parameters".
3.  Clique em "Add" na caixa "Interpreters".
4.  Você está agora na página para adicionar o interpretador ASP.NET. As duas opções que eu poderia recomendar são "FastCGI (Local - Pipes)" e "FastCGI (Remote - TCP/IP sockets)":
    -   **FastCGI (Local - Pipes)** - Abyss vai iniciar o servidor mono automaticamente usando um piped socket. Pipes são a maneira mais rápida de se comunicar e ter o Abyss repassando o seu próprio servidor, o que significa que você não terá que fazer isto manualmente. Esta é possivelmente a opção mais fácil.
        Se você estiver usando esta opção, Apenas configure o "Interpreter" para "/usr/bin/fastcgi-mono-server" ou "/usr/bin/fastcgi-mono-server2".
    -   **FastCGI (Remote - TCP/IP sockets)** - Abyss vai procurar pelo servidor mono em um endereço IP e porta específicos. Você pode usar isto para rodar o servidor em uma outra máquinae redistribuir o processo de carga. O único alerta é que você precisará iniciar o servidor mono manualmente em outro computador, usando um comando como "`fastcgi-mono-server2 /socket=tcp:8002`"
        Se estiver usando esta opção, simplesmente configure o "Remote server IP Address" para o IP da máquina que você estiver rodando o servidor do Mono, e a porta que você usou na linha de comando. Pela linha de comando que indicamos acima, esta deveria ser 8002.

5.  Desmarque a opção "Check for file existence before execution". Esta opão otimiza a performance mas pode corromper o ASP.NET 2.0 assim como algumas vezes usa caminhos que não necessáriamente existem como WebResource.axd.
6.  Desmarque a opção "Use the associated extensions to automatically update the Script Paths".
7.  Adicione "\*" à "Extensions". Isto não é uma verdade, mas será usado para garantir que todas as requisições chegem ao FastCGI do servidor Mono.
8.  Clique em "OK".
9.  Você deverá ser automaticamente direcionado para "Scripting Parameters". Clique em "Add" na caixa "Script Paths".
10. Entre "/\*" no "Virtual Path".
11. Clique "OK".

### Geral Passo 2: Extender o tempo de vida do servidor

Ao completar o passo anterior, você deve ter automaticamente retornado ao "Scripting Parameters". Clique em "Edit..." próximo a "FastCGI Parameters". A opção "FastCGI Processes Timeout" especifica o numero de segundos depois da ultima requisição você vai quere aguardar antes de desligar o Servidor Mono FasCGI(ou qualquer outro). Porque páginas ASP.NET precisam ser recompiladas a AppDomains precisam sere recriados toda a vez que o servidor inicia. Você configura este para um valor arbitrariamente alto. 604800 é um numero de segundos de uma semana e o valor que escolhi para meu servidor. Apenas após ter setado este valor clique em "OK". 


### Geral Passo 3: Desabilitando listar diretórios

Ao completar o passo anterior, você deverá ter automaticamente retornadoao "Scripting Parameters". Clique botão "OK" rodapá da página para voltar para a pagina de configuração do host. Apenas lá, clique em "Directory Listing" e configure "Type" para "Disabled". Se a listagem de diretórios estiver habilitada, os paths não serão automaticamente enviados para o servidor FastCGI Mono.

Usando Extensões
----------------

### Alertas de Extensões

**Usar extensões ao invés de caminhos NÃO é recomendado.** por favor consulte [FastCGI#Paths_vs._Extensions](/docs/web/fastcgi/#paths-vs-extensions) na página principal por uma explicação detalhada. Se você decidir usar esta configuração, por favor tenha em mente que é menos seguro, sofre desvantegens adicionais quando comparado com o uso de paths.

### Extenções Passo 1: Configurando o Interpretador

Ao iniciar o servidor web Abyss, um centro de controle do servidor web será iniciado por padrão no endereço
<http://localhost:9999>. Simplesmente abra seu navegador neste endereço e siga os passos abaixo:teps outlined below:

1.  Clique em "Configure" no host que você deseja adicionar suporte a ASP.NET
2.  Clique em "Scripting Parameters".
3.  Clique em "Add" na caixa "Interpreters".
4.  Você está agora na página para adicionar o interpretador ASP.NET. As duas opções que eu poderia recomendar são "FastCGI (Local - Pipes)" e "FastCGI (Remote - TCP/IP sockets)":
    -   **FastCGI (Local - Pipes)** - Abyss vai iniciar o servidor mono automaticamente usando um piped socket. Pipes são a maneira mais rápida de se comunicar e ter o Abyss repassando o seu próprio servidor, o que significa que você não terá que fazer isto manualmente. Esta é possivelmente a opção mais fácil.
        Se você estiver usando esta opção, Apenas configure o "Interpreter" para "/usr/bin/fastcgi-mono-server" ou "/usr/bin/fastcgi-mono-server2".
    -   **FastCGI (Remote - TCP/IP sockets)** - Abyss vai procurar pelo servidor mono em um endereço IP e porta específicos. Você pode usar isto para rodar o servidor em uma outra máquinae redistribuir o processo de carga. O único alerta é que você precisará iniciar o servidor mono manualmente em outro computador, usando um comando como "`fastcgi-mono-server2 /socket=tcp:8002`"
        Se estiver usando esta opção, simplesmente configure o "Remote server IP Address" para o IP da máquina que você estiver rodando o servidor do Mono, e a porta que você usou na linha de comando. Pela linha de comando que indicamos acima, esta deveria ser 8002.

5.  Desmarque a opção "Check for file existence before execution". Esta opão otimiza a performance mas pode corromper o ASP.NET 2.0 assim como algumas vezes usa caminhos que não necessáriamente existem como WebResource.axd.
6.  Adicione as extensões abaixo:
    -   aspx
    -   asmx
    -   ashx
    -   asax
    -   ascx
    -   soap
    -   rem
    -   axd
    -   cs
    -   config
    -   dll

7.  Clique em "OK".

### Extensões Passo 2: Extendendo o Tempo de vida do servidor

Ao completar o passo anterior, você deve ter automaticamente retornado ao "Scripting Parameters". Clique em "Edit..." próximo a "FastCGI Parameters". A opção "FastCGI Processes Timeout" especifica o numero de segundos depois da ultima requisição você vai quere aguardar antes de desligar o Servidor Mono FasCGI(ou qualquer outro). Porque páginas ASP.NET precisam ser recompiladas a AppDomains precisam sere recriados toda a vez que o servidor inicia. Você configura este para um valor arbitrariamente alto. 604800 é um numero de segundos de uma semana e o valor que escolhi para meu servidor. Apenas após ter setado este valor clique em "OK". 


### Extensões Passo 3: Adicionando Páginas Index

Ao completar o passo anterior,você deve automaticamente retornar para "Scripting Parameters". Clique em "OK" no rodapé da página para retornar para a página de configuração do host. Apenas lá, clique em "Index Files" e continue adicionando `index.aspx`, `default.aspx`, e `Default.aspx`. Clique em "OK" e entao em "Reset" para reiniciar o servidor.

Sucesso
-------
Agora você deve estar com ASP.NET funcionando com o Servidor Web Abys. Divirta-se!

