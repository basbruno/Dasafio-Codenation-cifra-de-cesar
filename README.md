# Dasafio-Codenation-cifra-de-cesar
Desafio da Cifra de Cesar Codenation, utilizando Google Colab Python#
https://colab.research.google.com/gist/dremeloke/0fba5d5ca468c1f4845308e84bfc507f/untitled14.ipynb

# -*- coding: utf-8 -*-
import json
import requests
import os
import hashlib



r = requests.get('https://api.codenation.dev/v1/challenge/dev-ps/generate-data?token=0d3d21d22772c3b860368399033867ea83f7aeb6')

resposta = r.json()
arquivo = open('answer.json', "w")
json.dump(resposta, arquivo)
arquivo.close()
msg = resposta['cifrado']

L2I = dict(zip("abcdefghijklmnopqrstuvwxyz",range(26)))
I2L = dict(zip(range(26),"abcdefghijklmnopqrstuvwxyz"))

key = 8
plaintext = msg

# defipher
ciphertext = ""
for c in plaintext.lower():
    if c.isalpha(): ciphertext += I2L[ (L2I[c] - key)%26 ]
    else: ciphertext += c

print (ciphertext)

dados = {
    'numero_casas': resposta['numero_casas'],
    'token': resposta['token'],
    'cifrado': resposta['cifrado'],
    'decifrado': ciphertext,
    'resumo_criptografico': hashlib.sha1(ciphertext.encode()).hexdigest()
}

print(dados)

arquivo = open('answer.json', "w")
json.dump(dados, arquivo)
arquivo.close()

url = 'https://api.codenation.dev/v1/challenge/dev-ps/submit-solution?token=0d3d21d22772c3b860368399033867ea83f7aeb6'
files = {'answer': open('answer.json', 'rb')}
r = requests.post(url, files=files)
r.text

