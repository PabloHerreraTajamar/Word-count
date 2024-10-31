
# WordCount-Hadoop

Práctica 1 - Levantar un clúster de Hadoop con los servicios de YARN, hacer una prueba. Por ejemplo: word count. 


## Montar el contenedor


Clonar el repositorio para tener ya todos los archivos

```bash
  git clone https://github.com/MarioSantanaTajamar/WordCount-Hadoop.git
```

Como ya esta el compose hecho, montamos el contenedor
```bash
  docker-compose up -d
```

## Acceder al namenode y crear el directorio user
Entra al contenedor
```bash
  docker exec -it namenode /bin/bash
```

Creamos la ruta donde va a ir el archivo que vamos a contar
```bash
  hdfs dfs -mkdir -p /user/root
```
Para ver el directorio creado
```bash
  hdfs dfs -ls /
```

## Proceso de mover el archivo y contarlo
Ya está el .jar, pero si lo quieres borrar y bajarlo este es el comando
```bash
   wget https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-mapreduce-examples/2.7.1/hadoop-mapreduce-examples-2.7.1-javadoc.jar
```
Copiamos el .jar en la ruta /tmp/ del namenode
```bash
  docker cp hadoop-mapreduce-examples-2.7.1-javadoc.jar namenode:/tmp/
```

Aquí ya tengo el archivo que voy a contar (nombreArchivo), pero lo puedes borrar y crear otro nuevo.
```bash
  nano nombreArchivo.txt
```
Contenido:
```bash
  La tecnología ha avanzado mucho en los últimos años. La tecnología ha cambiado cómo trabajamos, cómo nos comunicamos y cómo vivimos. La tecnología no solo es una herramienta, sino una parte fundamental de nuestras vidas. Las personas usan la tecnología para estudiar, para trabajar y para entretenerse. Hoy en día, la tecnología permite que las personas estén más conectadas. La tecnología también mejora el acceso a la información y facilita el aprendizaje. Sin embargo, la tecnología tiene desafíos, como el impacto en la privacidad y la dependencia de dispositivos. En definitiva, la tecnología es una fuerza transformadora, y la forma en que las personas usan la tecnología seguirá evolucionando.
```

Copiar el archivo creado del local al /tmp del namenode
```bash
  cp nombreArchivo.txt namenode:/tmp/
```
Para comprobar si está
```bash
  docker exec -it namenode /bin/bash
  cd /tmp/
  ls
```
Crear un directorio input en user para meter ahi el archivo
```bash
  hdfs dfs -mkdir /user/root/input
```
Mover el archivo a ese directorio
```bash
  hdfs dfs -put /tmp/nombreArchivo.txt input
  hdfs dfs -ls /user/root/input
```

Ya contamos
```bash
  hadoop jar hadoop-mapreduce-examples-2.7.1-javadoc.jar org.apache.hadoop.examples.WordCount input output
```

Para visualizar
```bash
  hdfs dfs -cat /user/root/output/*
```
