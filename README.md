# Desafio-Cibersegurança
Atividade do curso de Cibersegurança


<h1>1. Varredura de Portas (Reconhecimento)</h1>
<p>O primeiro comando executado foi: nmap -sV -p 21,22,80,445,139 192.168.56.101
O que o comando faz: O usuário utilizou a ferramenta Nmap para escanear portas específicas (FTP, SSH, HTTP, SMB) no endereço IP alvo.</p>
A flag <strong>-sV</strong>: Foi usada para tentar descobrir a versão dos serviços rodando nessas portas.
O Resultado: O scan confirmou as portas abertas e identificou versões de software antigas e conhecidas, como:

<ul>
  <li>Porta 21 (FTP): vsftpd 2.3.4</li>
  <li>Porta 80 (HTTP): Apache httpd 2.2.8</li>
  <li>Porta 139/445 (Samba): Samba 3.X - 4.X</li>
</ul>

<h2>2. Conexão ao Serviço (Enumeração)</h2>
<p>O segundo comando (o atual) é: ftp 192.168.56.101
Ação: Após ver que a porta 21 estava aberta no scan anterior, o usuário iniciou o cliente FTP para tentar se conectar ao servidor.</p>
Estado Atual: A conexão foi estabelecida (Connected to...). O servidor respondeu com o banner 220 (vsFTPd 2.3.4) e agora está aguardando o usuário digitar um nome de login na linha Name (192.168.56.101:carlos):.
<img width="1031" height="715" alt="Comandos no KaliLinux" src="https://github.com/user-attachments/assets/eebb193a-72bc-434f-8665-b68d76c012b7" />


<h1>Ataque de força bruta (brute force) contra serviço FTP</h1>
<ol> 
 <li> Preparação das Wordlists (Listas de Palavras)
Antes de atacar, eu criou dois arquivos de texto pequenos para servirem de "dicionário" para o ataque:

Primeiro comando: echo -e "user\nmsfadmin\n..." > user.txt

E criado um arquivo chamado user.txt contendo nomes de usuários prováveis (como admin, root, msfadmin).

Segundo comando: echo -e "123456\npassowrd\n..." > pass.txt

Cria um arquivo pass.txt com senhas comuns para testar.
</li>
<li> Execução do Ataque com Medusa
O comando principal executado foi: medusa -h 192.168.56.101 -U user.txt -P pass.txt -M ftp -t 6

-h: Define o Host (alvo).

-U e -P: Indicam os arquivos de usuários e senhas que você acabou de criar.

-M ftp: Especifica que o módulo de ataque é para o protocolo FTP.

-t 6: Define que a ferramenta fará 6 tentativas simultâneas (threads) para agilizar o processo.
</li>
<li> O Resultado (Sucesso!)
A ferramenta testou as combinações de usuário e senha. No meio da saída, há uma linha indicando sucesso:

ACCOUNT FOUND: [ftp] ... User: msfadmin Password: msfadmin [SUCCESS]

Conclusão: O ataque foi bem-sucedido. O Medusa descobriu que o usuário é msfadmin e a senha também é msfadmin.

Agora com as credenciais válidas, o próximo passo lógico seria tentar fazer o login no FTP (comando ftp 192.168.56.101) usando esses dados.</li>
</ol>
<img width="918" height="760" alt="Comando Medusa" src="https://github.com/user-attachments/assets/2edfdef0-dc0d-4ac3-9e4b-24f695d887ce" />



