### Подача gentx для проекта DEFUND (Не обязательно).Это для запуска отберут 75 валидаторов.

#### Рекомендуемые требования для сервера 16GB RAM, 4vCPUs, 200GB Disk space

```
sudo apt update && sudo apt upgrade -y
```

```
sudo apt install make clang pkg-config libssl-dev build-essential git gcc chrony curl jq ncdu bsdmainutils htop net-tools lsof fail2ban wget -y
```

#### Устанавливаем go и проверяем версию

```
ver="1.19.1" && \
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz" && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz" && \
rm "go$ver.linux-amd64.tar.gz" && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile && \
source $HOME/.bash_profile && \
go version
```

#### После этого скачиваем и устанавливаем бинарник

```
git clone https://github.com/defund-labs/defund
cd defund
git checkout v0.1.0-alpha
make install
cd $HOME
```

#### Задаем переменные (CHAIN оставляем без изменений, в остальные вписываем свои данные)

```
DEFUND_MONIKER="your_name"
DEFUND_CHAIN="defund-private-2"
DEFUND_WALLET="your_name"
```

#### Добавляем все в баш профиль

```
echo 'export DEFUND_MONIKER='${DEFUND_MONIKER} >> $HOME/.bash_profile
echo 'export DEFUND_CHAIN='${DEFUND_CHAIN} >> $HOME/.bash_profile
echo 'export DEFUND_WALLET='${DEFUND_WALLET} >> $HOME/.bash_profile
source $HOME/.bash_profile
```

#### Инициализируем ноду

```
defundd init $DEFUND_MONIKER --chain-id $DEFUND_CHAIN
```

#### Создаем кошелек (не забываем сохранить мнемоник)

```
defundd keys add $DEFUND_WALLET
```

#### Если кошелек уже есть то восстанавливаем с помощью мнемоника

```
defundd keys add $DEFUND_WALLET --recover
```

#### Добавляем адрес кошелька в переменную для нашего удобства

```
DEFUND_ADDR=$(defundd keys show $DEFUND_WALLET -a)
```

#### Добавляем переменную в баш

```
echo 'export DEFUND_ADDR='${DEFUND_ADDR} >> $HOME/.bash_profile
source $HOME/.bash_profile
```

#### Далее создаём генезис аккаунт

```
defundd add-genesis-account $DEFUND_ADDR 100000000ufetf
```

#### И создаём gentx

```
defundd gentx $DEFUND_WALLET 90000000ufetf \
--chain-id defund-private-2 \
--moniker=$DEFUND_MONIKER \
--commission-max-change-rate=0.01 \
--commission-max-rate=0.20 \
--commission-rate=0.05 
```

#### Если все прошло нормально то файл gentx будет сохранён по пути ${HOME}/.defundd/config/gentx/gentx-XXXXXXXX.json.

#### Теперь необходимо подать gentx, для этого переходим на гитхаб https://github.com/defund-labs/testnet
#### (у вас должен быть зарегистрированный аккаунт) и делаем форк репозитория

#### Как поправильно подать файл можно глятуть вот здесь https://nodes.mms.team/Defund_invited_gentx#8YeU (спасибо автору)

#### На этом все (напомню это не обязательный шаг)

### Мой ютуб канал -  https://www.youtube.com/channel/UCM3eD0Rh3a52c6NoFeCCYzg
