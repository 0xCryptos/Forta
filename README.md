**Установка производится из под root**

**Выполняем по очереди**

-
		sudo apt update && sudo apt upgrade -y  
-	
		apt install ca-certificates curl gnupg lsb-release git htop
-
		curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
-
		echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
-
		apt-get update
-
		apt-get install docker-ce docker-ce-cli containerd.io

Проверяем докер

-
		docker version

Version:           20.10.16 (или что-то похожее)

Открываем файл:

	nano /etc/docker/daemon.json

Вставляем туда все что в блоке 

	
	{
	   "default-address-pools": [
        {
            "base":"172.17.0.0/12",
            "size":16
        },
        {
            "base":"192.168.0.0/16",
            "size":20
        },
        {
            "base":"10.99.0.0/16",
            "size":24
        }
    ]
	}
	
 сохраняем, закрываем : **ctrl+S, ctrl+X**

-
		systemctl restart docker

Устанавливаем ноду

-
		sudo curl https://dist.forta.network/pgp.public -o /usr/share/keyrings/forta-keyring.asc -s
-
		echo 'deb [signed-by=/usr/share/keyrings/forta-keyring.asc] https://dist.forta.network/repositories/apt stable main' | sudo tee -a /etc/apt/sources.list.d/forta.list
-
		apt-get update
-
		apt-get install forta


Придумываем пароль (вводим вместо PASSWORD):

	forta init --passphrase PASSWORD

После чего мы получим информацию о адресе ноды:

	
	Scanner address: 0xAAA8C491232cB65a65FBf7F36b71220B3E695AAA

	Successfully initialized at /root/.forta

	

## Конфиги 
Подумайте в каком чейне будет работать ваша нода, *в Polygon много нод и реварды не очень*
**Изменить эту настройку будет нельзя**

	nano /root/.forta/config.yml

Удаляем все что есть и всавляем конфиг для выбранного чейна

 **Ethereum**:

    ######################################################
    chainId: 1
    
    scan:
      jsonRpc:
        url: http://your-node:8545
    
    trace:
      jsonRpc:
        url: http://your-node:8545
    ######################################################

 **BSC**:

    ######################################################
    chainId: 56
    
    scan:
      jsonRpc:
        url: https://bsc-dataseed.binance.org/
    
    trace:
      enabled: false
    
    ######################################################

 **Polygon**:

    ######################################################
    chainId: 137
    
    scan:
      jsonRpc:
        url: https://polygon-rpc.com/
    
    trace:
      enabled: false
    
    ######################################################

 **Avalanche**:

    ######################################################
    chainId: 43114
    
    scan:
      jsonRpc:
        url: https://api.avax.network/ext/bc/C/rpc
    
    trace:
      enabled: false
    
    ######################################################

 **Arbitrum**:

    ######################################################
    chainId: 42161
    
    scan:
      jsonRpc:
        url: https://arb1.arbitrum.io/rpc
    
    trace:
      enabled: false
    ######################################################

 **Optimism**:

    ######################################################
    chainId: 10
    
    scan:
      jsonRpc:
        url: https://mainnet.optimism.io
    
    trace:
      enabled: false
    ######################################################

 Сохраняем, закрываем **ctrl+S, ctrl+X**





Теперь, вам надо со своего метамаск, на котором лежат MATIC в сети Polygon
закинуть 0,1 матик на адрес вашей ноды который выдало на шаге инициализации 
после чего прописать адрес овнера

    forta register --owner-address сюда-вписываем-свой-метамаск --passphrase сюда-пароль

Результат сохраните, так же увидите ссылку по которой можно посмотреть статус

    systemctl enable forta

**Ставим tmux и запускавем ноду**

-
	    sudo apt install tmux
- 
		tmux new -s forta
-
		forta run --passphrase PASSWORD
Отключаемся от сесии оставив ее работать  **ctrl+B, D**
Подключиться к сесии снова можно командой:
	
	tmux attach



Шпарглка по tmux [тут](https://www.notion.so/Tmux-09c60cdf985e405187353c4fbb8b7a6f) 

Офф гайд по установке [тут](https://docs.forta.network/en/latest/scanner-quickstart/)

Гайд по регистрации ноды [тут](https://forta.notion.site/Forta-Fortification-Network-4a8af3ab4aea480d993e5095ad0ed746) 

Чекер [тут](https://asddsa.ru/forta/checker.html)
