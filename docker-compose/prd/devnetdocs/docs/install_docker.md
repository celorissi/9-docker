# Guia de Instalação do Docker no Oracle Linux

## **Passo 1: Atualizar o Sistema**
Antes de instalar o Docker, é importante garantir que o seu sistema esteja atualizado. Para isso, execute os comandos abaixo no terminal:

```bash
sudo yum update -y
```

---

## **Passo 2: Instalar o Docker no Oracle Linux**

### **2.1. Instalar dependências**
Instale pacotes necessários para que o Oracle Linux consiga instalar pacotes via HTTPS:

```bash
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

### **2.2. Adicionar o repositório oficial do Docker**
Adicione o repositório oficial do Docker ao Oracle Linux:

```bash
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

### **2.3. Instalar o Docker**
Instale o Docker CE, CLI e Containerd:

```bash
sudo yum install docker-ce docker-ce-cli containerd.io -y
```

### **2.4. Iniciar e habilitar o Docker**
Garanta que o Docker inicie automaticamente após reiniciar o sistema:

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

### **2.5. Verificar a instalação**
Verifique se o Docker foi instalado corretamente:

```bash
sudo docker --version
```

Teste a instalação executando o comando abaixo:

```bash
sudo docker run hello-world
```

---

## **Passo 3: Adicionar o Usuário ao Grupo Docker**
Para usar o Docker sem precisar de permissões `sudo`, adicione seu usuário ao grupo `docker`:

```bash
sudo usermod -aG docker $USER
```

Depois, saia e entre novamente para que as mudanças tenham efeito.

---

## **Passo 4: Criar o Dockerfile**
Agora, vamos criar um `Dockerfile` para sua aplicação.

1. Crie um diretório para o seu projeto:

   ```bash
   mkdir ~/meu-projeto
   cd ~/meu-projeto
   ```

2. Crie um arquivo chamado `Dockerfile` no diretório do seu projeto:

   ```bash
   nano Dockerfile
   ```

3. Adicione o conteúdo ao `Dockerfile`. Exemplo para uma aplicação Python:

   ```dockerfile
   # Usar uma imagem base do Python
   FROM python:3.9-slim

   # Definir o diretório de trabalho no container
   WORKDIR /app

   # Copiar os arquivos do projeto para o diretório de trabalho
   COPY . /app

   # Instalar dependências
   RUN pip install -r requirements.txt

   # Expor a porta em que o app vai rodar
   EXPOSE 5000

   # Comando para rodar o app
   CMD ["python", "app.py"]
   ```

---

## **Passo 5: Criar o Arquivo de Requisitos (se necessário)**
Se sua aplicação usa dependências, crie um arquivo `requirements.txt`:

1. Crie o arquivo:

   ```bash
   nano requirements.txt
   ```

2. Adicione as dependências necessárias para seu projeto. Exemplo para uma aplicação Flask:

   ```
   Flask==2.0.1
   ```

---

## **Passo 6: Construir a Imagem Docker**
Agora que você tem o `Dockerfile` e os arquivos necessários, construa a imagem Docker:

```bash
sudo docker build -t meu-projeto .
```

Verifique se a imagem foi criada com sucesso:

```bash
sudo docker images
```

---

## **Passo 7: Executar o Container**

Execute o container com base na imagem que você acabou de construir:

```bash
sudo docker run -d -p 5000:5000 meu-projeto
```

O comando `-d` faz o Docker rodar o container em segundo plano, e `-p 5000:5000` mapeia a porta 5000 do container para a porta 5000 do host.

Acesse a aplicação em um navegador: [http://localhost:5000](http://localhost:5000).

---

## **Passo 8: Parar e Remover Containers e Imagens**

### Parar o container:

```bash
sudo docker stop <container_id>
```

### Remover o container:

```bash
sudo docker rm <container_id>
```

### Remover a imagem:

```bash
sudo docker rmi meu-projeto
```

---

## **Passo 9: Monitorar Containers**

### Para visualizar os containers em execução:

```bash
sudo docker ps
```

### Para ver todos os containers, incluindo os parados:

```bash
sudo docker ps -a
```

---

## **Passo 10: Gerenciar Docker com Docker Compose (opcional)**
Se você tiver uma aplicação que precisa de vários containers, considere usar o Docker Compose.

### **10.1. Instalar o Docker Compose**

```bash
sudo yum install docker-compose -y
```

### **10.2. Criar o arquivo `docker-compose.yml`**
Exemplo de configuração para uma aplicação simples:

```yaml
version: "3"
services:
  web:
    build: .
    ports:
      - "5000:5000"
```

### **10.3. Iniciar os containers com o Docker Compose**

```bash
sudo docker-compose up
```

---

Com esses passos, você terá o Docker configurado no Oracle Linux e poderá gerenciar sua aplicação de forma eficiente!
