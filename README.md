# WriteUp_Deathnote
opa!!! Caio aka Choujin mais uma vez aqui, dessa vez com o write up da máquina Deathnote inspirada... bom... em Death Note

A máquina é catalogada como fácil, então vamos logo porque apesar de ser sim fácil foi uma máquina interessante

### Passo 1: Varredura

aqui é padrão, rodo sudo **arp-scan -l**

<img width="640" height="181" alt="image" src="https://github.com/user-attachments/assets/c2da9d3f-e412-46cd-b6f6-36daa9eb2b60" />

por padrão, já sabemos que a vítima é a de final 105

### Passo 2: Enumeração

aqui rodei **nmap -sC -sV -p-** pra utilizar de scripts NSE padrão, checar pela versão dos serviços ativos e enumerar em todas as portas disponíveis, obtive então o seguinte resultado:

<img width="667" height="283" alt="image" src="https://github.com/user-attachments/assets/aa222e0a-e85b-4cab-997e-1cb09c3ad772" />

uma porta ssh que não mexeremos por enquanto e uma porta http, vamos checar o que tem de bom nela 

<img width="1100" height="423" alt="image" src="https://github.com/user-attachments/assets/d7dcdd7a-985b-4da0-b699-9fe75340f6ff" />

hmm, um erro e a url mudou 

nesse caso, adicionarei deathnote.vuln como host em **/etc/hosts**

após isso

<img width="1100" height="537" alt="image" src="https://github.com/user-attachments/assets/c059625c-fc98-467f-838f-ac8cfbaec4c5" />

temos essa bela tela com o Kira de fundo!!!

### Passo 3: Exploit!!!! >:^)

sem muitas delongas, procuro por diretórios escondidos com o **gobuster dir -u http://url.alvo -w /wordlist**

<img width="732" height="447" alt="image" src="https://github.com/user-attachments/assets/3e480323-8af5-42fa-b22f-e92d9951875c" />

sempre é bom verificar as robots.txt!!!

<img width="235" height="81" alt="image" src="https://github.com/user-attachments/assets/6bda5242-5aba-42aa-81a7-1bf47c2b17e6" />

eu não disse? agora temos uma dica sobre uma jpg, vamos ver o que podemos obter com ela

<img width="1482" height="187" alt="image" src="https://github.com/user-attachments/assets/a3f1b4b7-277c-4a82-83f3-b0c041268b35" />

hmm, nada demais... talvez com curl?

<img width="583" height="186" alt="image" src="https://github.com/user-attachments/assets/0bf875b7-6d3b-4628-9700-25e764cadbcd" />

o pai do Light ajudou demais com essa, agora sabemos que em algum lugar temos um arquivo com um login

daqui, decidi procurar por vulnerabilidades no site utilizando do nikto, com **nikto -h url.alvo**

<img width="1583" height="537" alt="image" src="https://github.com/user-attachments/assets/5a6cdece-3e18-49f8-90bf-93e13947b9b0" />

pela saída, percebemos que existe um diretório de uploads do wordpress, vamos checar o que tem lá 

<img width="801" height="273" alt="image" src="https://github.com/user-attachments/assets/97c98a3a-78a3-4418-a98b-762a96aa926e" />

<img width="922" height="298" alt="image" src="https://github.com/user-attachments/assets/3dfc5dda-327e-43cb-9202-f8e1b8d474d9" />

interessantemente, dentro da pasta "07" achamos os seguintes arquivos, além de arquivos padrão inúteis:

<img width="697" height="721" alt="image" src="https://github.com/user-attachments/assets/47f20e8a-e5b6-42a2-af91-a453f83f0162" />

bom, já sabemos que user.txt se trata do login, possivelmente notes.txt se trata da senha? descobriremos agora mesmo, baixei os arquivos utilizando do **wget** e depois rodei **hydra -L wordlist -P wordlist [ip] [serviço]**

<img width="1645" height="227" alt="image" src="https://github.com/user-attachments/assets/43b8c681-3f68-46be-ad68-d6abe125aabb" />

está lá!! temos login e senha, então logamos com ssh!!!

<img width="1901" height="373" alt="image" src="https://github.com/user-attachments/assets/05090c4b-8563-4c57-9d19-0226d140d136" />

aqui foi um momento genuinamente interessante. Eu fiquei um pouco perdido no que aquilo poderia significar visto que é algo que eu nunca vi e após alguma pesquisa descobri que é simplesmnte BRAINFUCK. BRAIN. FUCK.

se foi o Kira que codificou isso, posso dizer que ele tem meios esotéricos realmente

pesquisei no google um decoder de Brainfuck e o resultado foi esse:

<img width="1392" height="516" alt="image" src="https://github.com/user-attachments/assets/9697e873-7c55-4878-b8fe-f184418daeb3" />

claro que não seria tão simples assim pegar o Kira, então continuemos com nossa investigação

decidi olhar ao redor nos diretórios base e achei algo interessante na /opt

<img width="607" height="417" alt="image" src="https://github.com/user-attachments/assets/77ced576-dc47-4d42-b65f-6abece466008" />

ao retornar ao diretório L e ir ao fake notebook rule achamos os seguintes arquivos:

<img width="748" height="138" alt="image" src="https://github.com/user-attachments/assets/058286c9-16e9-4957-9c65-f7bbbecd9062" />

um base64 talvez? vamos descobrir no cyberchef

<img width="1488" height="558" alt="image" src="https://github.com/user-attachments/assets/6009e437-d9a0-4d75-97dc-e8ecc581f067" />

do jeito que eu gosto, agora temos uma senha que nos vai ser útil depois

explorando mais um pouco, agora no diretório home, achei mais um diretório interessante

<img width="358" height="92" alt="image" src="https://github.com/user-attachments/assets/e1d80c07-697c-432a-9856-2c2b2fcbebfb" />

hmm, acesso negado... e se tentarmos logar como kira com a senha que obtivemos?

<img width="951" height="131" alt="image" src="https://github.com/user-attachments/assets/f943c536-0f90-418c-bda3-ab4c66782878" />

maravilha!! eu definitivamente amo base64

decodificando o arquivo temos:

<img width="947" height="107" alt="image" src="https://github.com/user-attachments/assets/5508999e-186d-4819-ae9d-c17aad9d24f6" />

mais um diretório que parece ser relevante, agora em /var

<img width="403" height="36" alt="image" src="https://github.com/user-attachments/assets/93a60b05-d0de-42fa-b40e-2fcee9e4dfd0" />

ou... não, né. Parece que é tarde demais pra Misa

aqui eu parei um pouco e pensei no que mais poderia ser feito, e cheguei à decisão de checar os privilégios do Kira e descobri que ele pode rodar sudo com privilégios de root

<img width="912" height="146" alt="image" src="https://github.com/user-attachments/assets/39d02101-f6ac-4399-980d-a083621b2566" />

o indicativo aqui é o (ALL:ALL) ALL, que basicamente significa que o Kira pode rodar qualquer comando, como qualquer usuário, em qualquer grupo, sudo incluso!!

aqui rodamos **sudo -i** ou **sudo su** pra acessar como root, e...

<img width="1003" height="282" alt="image" src="https://github.com/user-attachments/assets/678226d9-799e-4581-8f6a-604f39f6c880" />

tá lá!!! nada tão satisfatório quanto uma root flag :^))))

é isso, essa máquina foi verdadeiramente mais fácil do que as demais mas não deixou de ser proveitosa, eu sinto que toda máquina é um potencial de aprendizado e ver isso acontecendo em tempo real sempre vai ser muito positivo!! muito obrigado e até a próxima fellas!







































