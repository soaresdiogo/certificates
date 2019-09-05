# certificados
Criar um certificado com raíz via openssl

## Primeiro passo é criar um certificado raíz

Para gerar o seu certificado raíz, primeiro abra o seu terminal de preferência e execute o seguinte comando openssl;

```bash
openssl genrsa -des3 -out rootCA.key 4096
```

## Segundo passo é criar o certificado raíz assinado

```bash
openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 9125 -out rootCA.crt
```

Esse certificado .crt gerado é o certificado que você irá instalar no seu servidor como certificado raíz confiável

# Certificado de domínio

## Terceiro passo é criar o certificado para o seu domínio

```
openssl genrsa -out meudominio.com.br.key 2048
```

## Quarto passo é criar o certificado assinado  (csr)

**Importante:** O common Name deve ser diferente do gerado no certificado raíz, nesse caso o ideal é colocar o domínio como common name.

```
openssl req -new -key meudominio.com.br.key -out meudominio.com.br.csr
```

## Verificar o conteúdo do arquivo csr's.

```
openssl req -in meudominio.com.br.csr -noout -text
```

## Gerar o certificado usando o `meudominio.com.br` csr com a chave privada da raíz

```
openssl x509 -req -in meudominio.com.br.csr -CA rootCA.crt -CAkey rootCA.key -CAcreateserial -out meudominio.com.br.crt -days 9125 -sha256
```

## Verificar o conteúdo do certificado

```
openssl x509 -in meudominio.com.br.crt -text -noout
```

## Gerar o pfx do certificado

```
openssl pkcs12 -export -out meudominio.com.br.pfx -inkey meudominio.com.br.key -in meudominio.com.br.crt
```
