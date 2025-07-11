# 42 Cursus - net_practice

<img src="./assets/netpracticee.png" alt="completion-without-bonus-badge" align="left">

O projeto "net_practice" da 42 School é um desafio que oferece uma introdução prática ao mundo das redes. O seu principal propósito é permitir aos participantes configurar redes em pequena escala, abrangendo conceitos fundamentais, tais como endereçamento IPv4, máscaras de sub-rede e roteamento, tanto local quanto global. Por meio de uma interface de treinamento, é feita a prática de entender e solucionar desafios de rede em 10 níveis distintos com dificuldade escalonada. Este é o primeiro projeto que não possui bônus (pontuação máxima 100).

## Redes <img src="https://img.shields.io/badge/GRADE-100%2F100-success?logo=42&logoColor=fff" align="right"/>

### IPv4

É um endereço numérico constituído por 4 octetos (32 bits) de 0 a 255. Ao ser convertido para binário, podemos visualizar melhor:

- `192.168.1.0`

| 192 | 168 | 1 | 0 |
| :---: | :---: | :---: | :---: |
| 11000000 | 10101000 | 00000001 | 00000000 |

Esses endereços são divididos em classes de A a E, de acordo com o número do primeiro octeto, e definindo a quantidade máxima de computadores que podem ser conectados a uma rede:

| Classe | Range | Quantidade de Hosts |
| :---: | :---: | :--- |
| A | 1 a 127 | 2²⁴ hosts |
| B | 128 a 191 | 2¹⁶ hosts |
| C | 192 a 223 | 2⁸ hosts |
| D | 224 a 239 | _\<reservado>_ multicast / unicast / broadcast / anycast |
| E | 240 a 255 | _\<reservado>_ teste de novas tecnologias |

#### IPs Restritos

Existem alguns números que são privados e não podem ser utilizados:

- `0.x.x.x` (reservado pelo IANA)
- `10.x.x.x` (reservado pelo IANA)
- `172.16.0.0` a `172.31.255.255` (reservado pelo IANA)
- `192.168.x.x` (reservado pelo IANA)
- `127.x.x.x` \[Loopback]
- `169.254.x.x` \[APIPA]

### Subnet Mask

Serve para determinar quais intervalos de endereços IP fazem parte da mesma sub-rede.

#### Notação de Ponto Decimal

A mesma lógica de binário se aplica à máscara de rede, onde:

- `255.255.255.0`

| 255 | 255 | 255 | 0 |
| :---: | :---: | :---: | :---: |
| 11111111 | 11111111 | 11111111 | 00000000 |

No entanto, uma máscara nunca mistura os bits 1 e 0, ou seja, ele inicia com 1, mas a partir do primeiro 0, o 1 não poderá aparecer novamente. Dessa forma, temos um número limitado de máscaras:

Para ter a capacidade de enviar pacotes entre dois endereços IP, eles precisam fazer parte da mesma rede ou precisam estar conectados por um roteador que faça parte de ambas as sub-redes.

#### CIDR

É outra forma de representar uma máscara e se trata da soma dos bits 1 do binário da máscara.

Considerando a mesma máscara anterior `255.255.255.0`, convertida para binário ela é iniciada por um total de 24 bits `1`, e finalizada com 8 bits `0`, portanto pode ser representada simplesmente como `/24`.

### IP e Mask

Na máscara, os octetos preenchidos por bits 1 identificam a rede, e os bits 0 representam a identificação dos hosts. Por exemplo:

Considerando a máscara `255.255.128.0`, os dois primeiros octetos são completamente preenchidos por 1, significando que a parte que identifica a rede são os dois primeros octetos do IP. Supondo que o IP seja `130.10.1.1`, a parte que identifica a rede será `130.10`, e o restante é usado para subredes (por esse motivo cabem mais hosts na classe B). Neste caso, o "octeto misto" (aquele que define o CIDR) será o terceiro ao invés do último.

#### Tabela de Subnets (classe C)

| CIDR | Decimal | Binário | Total de Hosts | Total de Subredes |
| :---: | :---: | :---: | :---: | :---: |
| \32 | 255.255.255.255 | 11111111.11111111.11111111.11111111 | 0 | 256 |
| \31 | 255.255.255.254 | 11111111.11111111.11111111.11111110 | 0 | 128 |
| \30 | 255.255.255.252 | 11111111.11111111.11111111.11111100 | 2 | 64 |
| \29 | 255.255.255.248 | 11111111.11111111.11111111.11111000 | 6 | 32 |
| \28 | 255.255.255.240 | 11111111.11111111.11111111.11110000 | 14 | 16 |
| \27 | 255.255.255.224 | 11111111.11111111.11111111.11100000 | 30 | 8 |
| \26 | 255.255.255.192 | 11111111.11111111.11111111.11000000 | 62 | 4 |
| \25 | 255.255.255.128 | 11111111.11111111.11111111.10000000 | 126 | 2 |
| \24 | 255.255.255.0 | 11111111.11111111.11111111.00000000 | 254 | 1 |

> Do total de hosts é subtraído 2, pois o 255 é reservado para o broadcast, e normalmente o 0 é reservado para a rede.

Exemplo:

| Bits da Mask | Decimal | CIDR | Configuração |
| :---: | :---: | :---: | :---: |
| 11111100 | 254 | /30 | 64 subredes com (2 hosts + 1 rede + 1 broadcast) cada |

### Switch

É apenas um intermediário para conectar mais de dois dispositivos na mesma rede. Dessa forma, é necessário que todos os dispositivos conectados estejam dentro da mesma faixa de endereços.

### Router

Também se trata de um intermediário, desta vez para conectar várias redes entre si e possui uma interface para cada uma dessas redes.

Como o roteador separa redes diferentes, a faixa de possíveis endereços IP em uma de suas interfaces não deve se sobrepor com a faixa de suas outras interfaces. Uma sobreposição na faixa de endereços IP implicaria que as interfaces estão na mesma rede.

#### Router Table

Cada elemento, seja um roteador, computador ou internet, pode possuir uma tabela de roteamento, representados por um quadro roxo neste projeto. Ela armazena uma lista com rotas para redes despecíficas.

- O primeiro elemento a ser preenchido é o **destino**: Nele é especificado um endereço de rede no qual um host é o alvo final, e a máscara é representada no formato CIDR em seguida. Quando não há outra rota disponível, a rota padrão (default ou 0.0.0.0/0) é utilizada.
- O segundo elemento a ser preenchido é o **próximo salto**: Refere-se ao endereço IP do roteador mais próximo pelo qual um pacote pode passar.
