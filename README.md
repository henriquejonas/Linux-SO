Linux-SO
========

Instruções para compilação e instalação do kernel:
--------------------------------------------------

1. Baixar a versão mais estável no kernel no endereço: http://kernel.org. Como já temos a versão 3.11.5 aqui no repo, esse passo pode ser pulado
2. Copiar o conteúdo da pasta do kernel para /usr/src: sudo cp -r linux-3.11.5 /usr/src
3. Instalar o gcc (sudo apt-get install gcc) caso ainda não o tenha
4. Acessar a pasta: cd /usr/src/linux-3.11.5
5. Configurar o kernel: sudo make menuconfig. 
	1. Ao rodar esse comando vai abrir uma janela de configuração de geração do kernel. É só salvar a configuração atual, não precisa alterar nada.
6. Começar a compilação gerando uma imagem comprimida do kernel: sudo make (vai demorar umas 2hrs)
7. Começar a compilação dos módulos do kernel: sud make modules
8. Instalar os módulos do kernel: sudo make modules_install
9. Depois, instalar o kernel em si (pode demorar um pouco): sudo make install
	1. As vezes aparece um (warning/erro) mas pode ignorar, a instalação vai continuar normalmente
	2. Depois de instalar 3 arquivos serão gerados na pasta /boot:
		1. System.map-3.11.5
		2. config-3.11.5
		3. vmlinux-3.11.5
10. Agora é necessário criar a imagem do initrd (ela contém os device drivers para carregar o resto do sistema operacional).
	1. Comandos:
		1. cd /boot
		2. sudo mkinitramfs -o initrd.img-3.11.5 
11. Agora precisamos criar uma entrada no grub:
	1. Para facilitar vamos instalar o grub-customizer:
		1. sudo add-apt-repository ppa:danielrichter2007/grub-customizer
		2. sudo apt-get update
		3. sudo apt-get install grub-customizer

	2. Executar o software: gksu grub-customizer
	3. Depois de entrar nele, ir no menu -> new
		1. Em type selecionar Linux
		2. Em partition selecionar a partição que está a sua atual distribuição
		3. Em initial ramdisk colocar: /boot/initrd.img-3.11.5
		4. Em Linux image colocar: /boot/vmlinuz-3.11.5
		5. Selecionar OK e dar um nome a entrada
	4. Clicar em salvar e esperar o software atualizar o grub
12. Reiniciar a máquina: sudo reboot
13. Ao entrar na tela do grub, selecionar a entrada criada
	1. Deve aparecer uma mensagem de: "no args....". É só apertar ENTER e esperar carregar os módulos do kernel

##### Finalmente para verificar se o kernel está instalando é só abrir o terminal (ou Ctrl + F1) e digitar: uname -a ou cat /proc/version
