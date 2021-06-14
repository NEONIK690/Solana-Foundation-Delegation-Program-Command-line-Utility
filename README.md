# Solana-Foundation-Delegation-Program-Command-line-Utility
## Привязка ключа ТДС к вновь создаваемому ключу Майнета

Перед началом связывания ключей у вас должна быть установлена сама нода Соланы и вы уже имеете свой pubkey из ТДС

```
apt-get update
sudo apt-get -y install libssl-dev libudev-dev pkg-config zlib1g-dev llvm clang make
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

вводим цифру 1 и жмем enter

```
source $HOME/.cargo/env
cargo install solana-foundation-delegation-program-cli
```
После этого переходим в папку solana и работаем в ней.

Создадим новый ключ для Майнета используя команду ниже:
```
solana-keygen new -o mainnet-validator-keypair.json
```
Убедитесь что ключ создался в папке солана. Это можно сделать командой ls -al
**Обязательно запишите мнемоническую фразу для восстановления вашего ключа и храните ее в надежном месте.**
**Также сохраняем файл json вашего нового ключа майнета.**

Переходим в кошелек где у вас есть SOL или покупаем токен на бирже и отправляем минимум 0.003 сол 
на ваш адрес ключа майнет. С него будет списана комиссия при связывании ключей.


После успешного пополнения баланса необходимо внести изменения в конфиг для того чтобы переназначить списание средств на ключ майнета
делаем это с помощью:

`solana config set --keypair ~/solana/mainnet-validator-keypair.json` 

(у вас путь к файлу mainnet-validator-keypair.json может отличаться!!! вы должны
прописать именно свой путь к файлу)

После этого делаем привязку ключей:

```
solana-foundation-delegation-program apply --mainnet ~/solana/mainnet-validator-keypair.json --testnet ~/solana/validator-keypair.json  --confirm
```

mainnet-validator-keypair.json и validator-keypair.json это имена файлов! Они могут отличаться у вас. 
Это маловероятно, но все равно стоит на это обратить внимание.

Также ~/solana/mainnet-validator-keypair.json и ~/solana/validator-keypair.json это пути до ваших ключей, они тоже могут быть другими.

Если все прошло успешно вы увидите строки с вашими ключами и строку связки
```
Mainnet Validator Identity: Y6QMPrYtZLsKTuT8mtYBD9pj1h3wrqUhJKCFDv1L26NR
Testnet Validator Identity: E1bcuniYQhDscfDjE4zaAXQ51TH77gsQoRuzvqXHxnDf
6txERiviWdXG6BeyRQD3b4RN1v3LZd94khK4vxEz2TNeYSzoLmkDNsMKdkC6rVEwUe5kCUbS8tGmYq2vZa4fTJi
```
solana-foundation-delegation-program status ~/solana/mainnet-validator-keypair.json  -этой командой можно проверить состояние вашей связки ключей (пути могут отличаться).
Как только статус "Pending" сменится на "Aprove" можно радоваться - вас одобрили в майнет.
