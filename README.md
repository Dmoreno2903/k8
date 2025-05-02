# Kubernetes
## Instalación de minikube u orbstack
## Configuración de la base de datos
Desplegamos el manifiesto encargado de la base de datos
```shell
kubectl apply -k overlay/development/database
```
Esto nos crea un servicio de `postgres` con un pod al que podemos acceder por el puerto `30432`
> Crear las bases de datos requeridas y sus backups correspondientes
## Instalación de los repositorios y creación de imágenes en docker
Creamos una carpeta base llamada `instances`, donde clonamos los repositorios y preparamos carpetas compartidas:
```shell
mkdir instances && cd instances
mkdir static logs media
```
### Originabot
```shell
git clone git@gitlab.com:unergy-dev/origina-bot.git
cd origina-bot
docker build -t originabot:v1 .
```
Dentro de `overlay/development/auth/kustomization.yaml` reemplazamos 
```yaml
images:
- name: auth
  newName: auth
  newTag: v1
```

### Auth
```shell
git clone git@gitlab.com:unergy-dev/authentication-system.git
cd authentication-system
docker build -t auth:v1 .
```
Dentro de `overlay/development/auth/kustomization.yaml` reemplazamos 
```yaml
images:
- name: auth
  newName: auth
  newTag: v1
```

> El nombre de las imágenes y su etiqueta se deben colocar en el `kustomize.yaml` de cada servicio
Asegurate de usar el mismo nombre y etiqueta de la imágen creada

## Variables de entorno
> Los archivos .env (Se los debe suministrar un desarrollador) se deben colorar:
### Auth
Debemos colocar el `.env` dentro de la carpeta `overlay/development/auth`
```plaintext
├── overlay/development/auth
|   ├── kustomization.yaml
|   ├── .env
```
### Originabot
Debemos colocar el `.env` dentro de la carpeta `overlay/development/originabot`
```plaintext
├── overlay/development/originabot
|   ├── kustomization.yaml
|   ├── .env
```
## Despliegue
Corremos el resto de manifiestos
```shell
kubectl apply -k overlay/development
```