
# README - Enviando Raster para PostgreSQL

## Requisitos

- PowerShell instalado no seu computador
- Acesso ao servidor remoto via SFTP e SSH
- `raster2pgsql` e `psql` instalados no servidor remoto

## Passos para Enviar e Importar Raster

### 1. Acesse o PowerShell

- Abra o PowerShell no seu computador.
- Digite `PowerShell` na barra de pesquisa do Windows e selecione o aplicativo.

### 2. Navegue até o Diretório do Arquivo ZIP

- No PowerShell, vá até o diretório onde o arquivo `.zip` está localizado.
  
  ```powershell
  cd C:\Temp
  ```

### 3. Acesse o Servidor Vênus via SFTP

- Use o comando SFTP para acessar o servidor:

  ```powershell
  sftp fribeiro@venus.iocasta.com.br
  ```

### 4. Faça o Upload do Arquivo ZIP

- No prompt SFTP, faça o upload do arquivo ZIP:

  ```sftp
  put seu_arquivo.zip
  ```

### 5. Acesse o Servidor via SSH

- Depois de fazer o upload, acesse o servidor via SSH:

  ```powershell
  ssh fribeiro@...
  ```

### 6. Verifique o Upload do Arquivo

- Liste os arquivos no servidor para confirmar que o upload foi bem-sucedido:

  ```bash
  ls
  ```

### 7. Descompacte o Arquivo ZIP

- Navegue até o diretório onde o arquivo ZIP foi enviado e descompacte-o:

  ```bash
  unzip seu_arquivo.zip
  ```

### 8. Envie o Raster para o Banco PostgreSQL

#### Parte 01: Crie o Arquivo SQL para Importação

- Gere o arquivo SQL usando `raster2pgsql`:

  ```bash
  raster2pgsql -s 4326 -I -C -M -F -t 100x100 teste.tif geo.teste > raster_import.sql
  ```

#### Parte 02: Importe o Arquivo SQL para o Banco de Dados

- Use o comando `psql` para importar o arquivo SQL:

  ```bash
  PGPASSWORD=... psql -h ... -p ... -U ... -d dblocation -f raster_import.sql
  ```

### Observações Importantes

- **Para importar um segundo raster em uma tabela já existente**, remova o `-C` e adicione o `-a`:

  ```bash
  raster2pgsql -s 4326 -I -M -F -t 30x30 -a brasil_coverage_2020.tif geo.raster > brasil_coverage_2020.sql
  PGPASSWORD=... psql -h ... -p 25432 -U ... -d ... -f brasil_coverage_2020.sql
  ```

  ```bash
  raster2pgsql -s 4326 -I -M -F -t 30x30 -a brasil_coverage_2021.tif geo.raster > brasil_coverage_2021.sql
  PGPASSWORD=yhwnaZ69mhpeDri psql -h postgres.iocasta.com.br -p 25432 -U postgres -d dblocation -f brasil_coverage_2021.sql
  ```
