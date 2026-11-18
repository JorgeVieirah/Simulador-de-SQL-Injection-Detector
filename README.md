# Simulador de SQL Injection: ataque e detecção

Laboratório didático para entender **SQL Injection** dos dois lados: como o ataque funciona e como uma aplicação pode se defender e registrar as tentativas.

> ⚠️ Uso exclusivamente educacional, em ambiente local. Não use estas técnicas contra sistemas sem autorização.

## O que tem aqui

| Arquivo | Papel |
|---|---|
| `app_com_sec.py` | Aplicação Flask com login e cadastro, usando SQLite. As consultas usam **parâmetros (`?`)**, o que neutraliza a injeção, e uma camada de **detecção** registra entradas suspeitas em `suspicious_logs.log`. |
| `atack.py` | Script de ataque que envia payloads clássicos (ex.: `admin' OR '1'='1' --`) para o login e mostra se o ataque teve sucesso. |

## Como funciona a defesa

1. **Prevenção:** consultas parametrizadas. O banco trata o que o usuário digita como dado, nunca como parte do comando SQL.
2. **Detecção:** antes de consultar o banco, a entrada é comparada com padrões comuns de injeção (`--`, `#`, ` OR `). Se houver correspondência, a tentativa é registrada com IP e horário.

Assim o ataque falha **e** fica um rastro para análise, que é a lógica de um time de defesa (Blue Team).

## Como executar

```bash
pip install -r requirements.txt

# terminal 1: sobe a aplicação em http://127.0.0.1:5000
python app_com_sec.py

# terminal 2: executa os ataques
python atack.py

# depois, veja as tentativas registradas
cat suspicious_logs.log
```

## O que aprendi / próximos passos

- [x] Diferença entre concatenar strings e usar consultas parametrizadas
- [x] Registro de eventos suspeitos para análise posterior
- [ ] Armazenar senhas com hash (bcrypt/argon2) em vez de texto puro
- [ ] Não registrar a senha digitada no log, apenas o usuário e o padrão detectado
- [ ] Versão "vulnerável" separada, para comparar os dois comportamentos lado a lado
- [ ] Enviar os logs para um SIEM (ex.: Splunk) e criar um alerta

## Stack

Python · Flask · SQLite · requests · logging
